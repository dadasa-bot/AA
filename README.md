local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local localPlayer = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- Settings
local lockKey = Enum.KeyCode.Q
local defaultLockDistance = 1000
local defaultSmoothness = 0.2
local defaultPrediction = 0.5 -- 0 = no prediction, 1 = full prediction
local defaultFOV = 70

-- State
local lockedPlayer = nil
local lockConnection = nil
local isEnabled = true
local currentLockDistance = defaultLockDistance
local currentSmoothness = defaultSmoothness
local currentPrediction = defaultPrediction
local targetVelocity = Vector3.new(0, 0, 0)
local lastTargetPosition = nil
local lastUpdateTime = tick()

-- Create main GUI
local gui = Instance.new("ScreenGui")
gui.Name = "dadasa"
gui.ResetOnSpawn = false
gui.Parent = localPlayer:WaitForChild("PlayerGui")

-- Main frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 320, 0, 250)
mainFrame.Position = UDim2.new(0.5, -160, 0.5, -125)
mainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 40)
mainFrame.BackgroundTransparency = 0.2
mainFrame.Active = true
mainFrame.Draggable = true

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 8)
corner.Parent = mainFrame

-- Title bar
local titleBar = Instance.new("Frame")
titleBar.Name = "TitleBar"
titleBar.Size = UDim2.new(1, 0, 0, 30)
titleBar.Position = UDim2.new(0, 0, 0, 0)
titleBar.BackgroundColor3 = Color3.fromRGB(50, 50, 70)
titleBar.Active = true

local titleCorner = Instance.new("UICorner")
titleCorner.CornerRadius = UDim.new(0, 8)
titleCorner.Parent = titleBar

local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, -40, 1, 0)
titleLabel.Position = UDim2.new(0, 10, 0, 0)
titleLabel.Text = "dadasa"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextXAlignment = Enum.TextXAlignment.Left
titleLabel.BackgroundTransparency = 1
titleLabel.Parent = titleBar

-- Close button
local closeButton = Instance.new("TextButton")
closeButton.Name = "CloseButton"
closeButton.Size = UDim2.new(0, 30, 0, 30)
closeButton.Position = UDim2.new(1, -30, 0, 0)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.Parent = titleBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 8)
closeCorner.Parent = closeButton

-- Toggle button
local toggleButton = Instance.new("TextButton")
toggleButton.Name = "ToggleButton"
toggleButton.Size = UDim2.new(0.9, 0, 0, 40)
toggleButton.Position = UDim2.new(0.05, 0, 0.15, 0)
toggleButton.Text = "ENABLE LOCK"
toggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
toggleButton.BackgroundColor3 = Color3.fromRGB(80, 180, 80)
toggleButton.Parent = mainFrame

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0, 6)
toggleCorner.Parent = toggleButton

-- Settings panel
local settingsFrame = Instance.new("Frame")
settingsFrame.Name = "SettingsFrame"
settingsFrame.Size = UDim2.new(0.9, 0, 0, 140)
settingsFrame.Position = UDim2.new(0.05, 0, 0.35, 0)
settingsFrame.BackgroundTransparency = 1
settingsFrame.Parent = mainFrame

-- Distance slider
local distanceLabel = Instance.new("TextLabel")
distanceLabel.Name = "DistanceLabel"
distanceLabel.Size = UDim2.new(1, 0, 0, 20)
distanceLabel.Position = UDim2.new(0, 0, 0, 0)
distanceLabel.Text = "Lock Distance: " .. defaultLockDistance
distanceLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
distanceLabel.TextXAlignment = Enum.TextXAlignment.Left
distanceLabel.BackgroundTransparency = 1
distanceLabel.Parent = settingsFrame

local distanceSlider = Instance.new("TextBox")
distanceSlider.Name = "DistanceSlider"
distanceSlider.Size = UDim2.new(1, 0, 0, 20)
distanceSlider.Position = UDim2.new(0, 0, 0, 20)
distanceSlider.Text = tostring(defaultLockDistance)
distanceSlider.PlaceholderText = "Enter lock distance"
distanceSlider.TextColor3 = Color3.fromRGB(0, 0, 0)
distanceSlider.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
distanceSlider.Parent = settingsFrame

-- Smoothness slider
local smoothnessLabel = Instance.new("TextLabel")
smoothnessLabel.Name = "SmoothnessLabel"
smoothnessLabel.Size = UDim2.new(1, 0, 0, 20)
smoothnessLabel.Position = UDim2.new(0, 0, 0, 45)
smoothnessLabel.Text = "Smoothness: " .. string.format("%.2f", defaultSmoothness)
smoothnessLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
smoothnessLabel.TextXAlignment = Enum.TextXAlignment.Left
smoothnessLabel.BackgroundTransparency = 1
smoothnessLabel.Parent = settingsFrame

local smoothnessSlider = Instance.new("TextBox")
smoothnessSlider.Name = "SmoothnessSlider"
smoothnessSlider.Size = UDim2.new(1, 0, 0, 20)
smoothnessSlider.Position = UDim2.new(0, 0, 0, 65)
smoothnessSlider.Text = string.format("%.2f", defaultSmoothness)
smoothnessSlider.PlaceholderText = "Enter smoothness (0-1)"
smoothnessSlider.TextColor3 = Color3.fromRGB(0, 0, 0)
smoothnessSlider.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
smoothnessSlider.Parent = settingsFrame

-- Prediction slider
local predictionLabel = Instance.new("TextLabel")
predictionLabel.Name = "PredictionLabel"
predictionLabel.Size = UDim2.new(1, 0, 0, 20)
predictionLabel.Position = UDim2.new(0, 0, 0, 90)
predictionLabel.Text = "Prediction: " .. string.format("%.2f", defaultPrediction)
predictionLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
predictionLabel.TextXAlignment = Enum.TextXAlignment.Left
predictionLabel.BackgroundTransparency = 1
predictionLabel.Parent = settingsFrame

local predictionSlider = Instance.new("TextBox")
predictionSlider.Name = "PredictionSlider"
predictionSlider.Size = UDim2.new(1, 0, 0, 20)
predictionSlider.Position = UDim2.new(0, 0, 0, 110)
predictionSlider.Text = string.format("%.2f", defaultPrediction)
predictionSlider.PlaceholderText = "Enter prediction (0-1)"
predictionSlider.TextColor3 = Color3.fromRGB(0, 0, 0)
predictionSlider.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
predictionSlider.Parent = settingsFrame

-- Lock indicator
local lockIndicator = Instance.new("Frame")
lockIndicator.Name = "LockIndicator"
lockIndicator.Size = UDim2.new(0, 20, 0, 20)
lockIndicator.Position = UDim2.new(0.5, -10, 0.5, -10)
lockIndicator.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
lockIndicator.BackgroundTransparency = 0.7
lockIndicator.Visible = false
lockIndicator.ZIndex = 10

local indicatorCorner = Instance.new("UICorner")
indicatorCorner.CornerRadius = UDim.new(1, 0)
indicatorCorner.Parent = lockIndicator

lockIndicator.Parent = gui

-- Parent all elements
titleBar.Parent = mainFrame
mainFrame.Parent = gui

-- Calculate target velocity for prediction
local function calculateVelocity(currentPosition)
    local now = tick()
    local deltaTime = now - lastUpdateTime
    
    if lastTargetPosition and deltaTime > 0 then
        targetVelocity = (currentPosition - lastTargetPosition) / deltaTime
    else
        targetVelocity = Vector3.new(0, 0, 0)
    end
    
    lastTargetPosition = currentPosition
    lastUpdateTime = now
    return targetVelocity
end

-- Find target function with prediction
local function findTarget()
    if not isEnabled then return nil end
    
    local closestPlayer = nil
    local closestAngle = math.rad(5) -- 5 degree cone
    
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Character then
            local humanoidRootPart = player.Character:FindFirstChild("HumanoidRootPart")
            if humanoidRootPart then
                -- Calculate predicted position
                local velocity = humanoidRootPart.AssemblyLinearVelocity
                local predictedPosition = humanoidRootPart.Position + (velocity * currentPrediction * 0.5)
                
                local direction = (predictedPosition - camera.CFrame.Position).Unit
                local angle = math.acos(camera.CFrame.LookVector:Dot(direction))
                
                if angle < closestAngle then
                    local distance = (humanoidRootPart.Position - camera.CFrame.Position).Magnitude
                    if distance <= currentLockDistance then
                        closestAngle = angle
                        closestPlayer = player
                    end
                end
            end
        end
    end
    
    return closestPlayer
end

-- Camera lock function with prediction
local function updateCameraLock()
    if lockedPlayer and lockedPlayer.Character then
        local humanoidRootPart = lockedPlayer.Character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            -- Calculate predicted position
            local velocity = humanoidRootPart.AssemblyLinearVelocity
            local predictedPosition = humanoidRootPart.Position + (velocity * currentPrediction * 0.3)
            
            calculateVelocity(humanoidRootPart.Position)
            
            local currentCFrame = camera.CFrame
            local targetPosition = predictedPosition
            
            -- Smooth transition
            local newLookVector = (targetPosition - currentCFrame.Position).Unit
            local smoothedLookVector = currentCFrame.LookVector:Lerp(newLookVector, currentSmoothness)
            
            camera.CFrame = CFrame.new(currentCFrame.Position, currentCFrame.Position + smoothedLookVector)
            return true
        end
    end
    return false
end

-- Toggle lock system
local function toggleLockSystem()
    if lockConnection then
        lockConnection:Disconnect()
        lockConnection = nil
        lockedPlayer = nil
        lockIndicator.Visible = false
        toggleButton.Text = "ENABLE LOCK"
        toggleButton.BackgroundColor3 = Color3.fromRGB(80, 180, 80)
    else
        lockedPlayer = findTarget()
        if lockedPlayer then
            lockIndicator.Visible = true
            toggleButton.Text = "DISABLE LOCK"
            toggleButton.BackgroundColor3 = Color3.fromRGB(180, 80, 80)
            
            lockConnection = RunService.RenderStepped:Connect(function()
                if not updateCameraLock() then
                    toggleLockSystem() -- Target lost
                end
            end)
        end
    end
end

-- Toggle enabled state
local function toggleEnabled()
    isEnabled = not isEnabled
    if isEnabled then
        toggleButton.Text = "ENABLE CAMERA LOCK"
        toggleButton.BackgroundColor3 = Color3.fromRGB(80, 180, 80)
    else
        toggleButton.Text = "DISABLE CAMERA LOCK"
        toggleButton.BackgroundColor3 = Color3.fromRGB(180, 80, 80)
        if lockConnection then
            toggleLockSystem()
        end
    end
end

-- UI Events
closeButton.MouseButton1Click:Connect(function()
    gui:Destroy()
end)

toggleButton.MouseButton1Click:Connect(toggleEnabled)

distanceSlider.FocusLost:Connect(function()
    local num = tonumber(distanceSlider.Text)
    if num and num > 0 then
        currentLockDistance = num
        distanceLabel.Text = "Lock Distance: " .. math.floor(num)
    else
        distanceSlider.Text = tostring(currentLockDistance)
    end
end)

smoothnessSlider.FocusLost:Connect(function()
    local num = tonumber(smoothnessSlider.Text)
    if num and num >= 0 and num <= 1 then
        currentSmoothness = num
        smoothnessLabel.Text = "Smoothness: " .. string.format("%.2f", num)
    else
        smoothnessSlider.Text = string.format("%.2f", currentSmoothness)
    end
end)

predictionSlider.FocusLost:Connect(function()
    local num = tonumber(predictionSlider.Text)
    if num and num >= 0 and num <= 1 then
        currentPrediction = num
        predictionLabel.Text = "Prediction: " .. string.format("%.2f", num)
    else
        predictionSlider.Text = string.format("%.2f", currentPrediction)
    end
end)

-- Keybind
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == lockKey and isEnabled then
        toggleLockSystem()
    end
end)

-- Cleanup
localPlayer.CharacterAdded:Connect(function()
    if lockConnection then
        toggleLockSystem() -- Reset when local player respawns
    end
end)
