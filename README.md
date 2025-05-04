-- Services
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local TextService = game:GetService("TextService")

-- Player references
local player = Players.LocalPlayer
local camera = workspace.CurrentCamera

-- Create main GUI
local screenGui = Instance.new("ScreenGui")
screenGui.ResetOnSpawn = false
screenGui.Name = "AdvancedCamLockUI"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Main container frame
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 220, 0, 160)
mainFrame.Position = UDim2.new(0.02, 0, 0.7, 0)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BackgroundTransparency = 0.4
mainFrame.Active = true
mainFrame.Draggable = true

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 8)
mainCorner.Parent = mainFrame

local mainStroke = Instance.new("UIStroke")
mainStroke.Color = Color3.fromRGB(80, 80, 80)
mainStroke.Thickness = 1
mainStroke.Parent = mainFrame

-- Title label
local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "TitleLabel"
titleLabel.Size = UDim2.new(1, 0, 0, 30)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.Text = "ADVANCED CAM LOCK"
titleLabel.TextColor3 = Color3.new(1, 1, 1)
titleLabel.TextSize = 14
titleLabel.Font = Enum.Font.GothamBold
titleLabel.BackgroundTransparency = 1
titleLabel.Parent = mainFrame

-- Lock button
local lockButton = Instance.new("TextButton")
lockButton.Name = "LockButton"
lockButton.Size = UDim2.new(0.9, 0, 0, 35)
lockButton.Position = UDim2.new(0.05, 0, 0.25, 0)
lockButton.Text = "LOCK: OFF"
lockButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
lockButton.TextColor3 = Color3.new(1, 1, 1)
lockButton.TextSize = 13

local lockCorner = Instance.new("UICorner")
lockCorner.CornerRadius = UDim.new(0, 6)
lockCorner.Parent = lockButton

local lockStroke = Instance.new("UIStroke")
lockStroke.Color = Color3.fromRGB(100, 100, 100)
lockStroke.Thickness = 1
lockStroke.Parent = lockButton
lockButton.Parent = mainFrame

-- Settings frame
local settingsFrame = Instance.new("Frame")
settingsFrame.Name = "SettingsFrame"
settingsFrame.Size = UDim2.new(0.9, 0, 0, 60)
settingsFrame.Position = UDim2.new(0.05, 0, 0.55, 0)
settingsFrame.BackgroundTransparency = 1
settingsFrame.Parent = mainFrame

-- Smoothness slider
local smoothnessLabel = Instance.new("TextLabel")
smoothnessLabel.Name = "SmoothnessLabel"
smoothnessLabel.Size = UDim2.new(0.4, 0, 0, 15)
smoothnessLabel.Position = UDim2.new(0, 0, 0, 0)
smoothnessLabel.Text = "Smoothness: 0.3"
smoothnessLabel.TextColor3 = Color3.new(0.8, 0.8, 0.8)
smoothnessLabel.TextSize = 11
smoothnessLabel.Font = Enum.Font.Gotham
smoothnessLabel.TextXAlignment = Enum.TextXAlignment.Left
smoothnessLabel.BackgroundTransparency = 1
smoothnessLabel.Parent = settingsFrame

local smoothnessSlider = Instance.new("Frame")
smoothnessSlider.Name = "SmoothnessSlider"
smoothnessSlider.Size = UDim2.new(1, 0, 0, 5)
smoothnessSlider.Position = UDim2.new(0, 0, 0, 18)
smoothnessSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

local smoothnessFill = Instance.new("Frame")
smoothnessFill.Name = "Fill"
smoothnessFill.Size = UDim2.new(0.3, 0, 1, 0)
smoothnessFill.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
smoothnessFill.Parent = smoothnessSlider

local smoothnessCorner = Instance.new("UICorner")
smoothnessCorner.CornerRadius = UDim.new(1, 0)
smoothnessCorner.Parent = smoothnessSlider
smoothnessSlider.Parent = settingsFrame

-- Jump offset slider
local jumpOffsetLabel = Instance.new("TextLabel")
jumpOffsetLabel.Name = "JumpOffsetLabel"
jumpOffsetLabel.Size = UDim2.new(0.4, 0, 0, 15)
jumpOffsetLabel.Position = UDim2.new(0, 0, 0, 30)
jumpOffsetLabel.Text = "Jump Offset: 1.5"
jumpOffsetLabel.TextColor3 = Color3.new(0.8, 0.8, 0.8)
jumpOffsetLabel.TextSize = 11
jumpOffsetLabel.Font = Enum.Font.Gotham
jumpOffsetLabel.TextXAlignment = Enum.TextXAlignment.Left
jumpOffsetLabel.BackgroundTransparency = 1
jumpOffsetLabel.Parent = settingsFrame

local jumpOffsetSlider = Instance.new("Frame")
jumpOffsetSlider.Name = "JumpOffsetSlider"
jumpOffsetSlider.Size = UDim2.new(1, 0, 0, 5)
jumpOffsetSlider.Position = UDim2.new(0, 0, 0, 48)
jumpOffsetSlider.BackgroundColor3 = Color3.fromRGB(60, 60, 60)

local jumpOffsetFill = Instance.new("Frame")
jumpOffsetFill.Name = "Fill"
jumpOffsetFill.Size = UDim2.new(0.5, 0, 1, 0)
jumpOffsetFill.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
jumpOffsetFill.Parent = jumpOffsetSlider

local jumpOffsetCorner = Instance.new("UICorner")
jumpOffsetCorner.CornerRadius = UDim.new(1, 0)
jumpOffsetCorner.Parent = jumpOffsetSlider
jumpOffsetSlider.Parent = settingsFrame

-- Prediction display
local predictionFrame = Instance.new("Frame")
predictionFrame.Name = "PredictionFrame"
predictionFrame.Size = UDim2.new(0, 100, 0, 20)
predictionFrame.Position = UDim2.new(0.5, -50, 0.85, 0)
predictionFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
predictionFrame.BackgroundTransparency = 0.5

local predictionCorner = Instance.new("UICorner")
predictionCorner.CornerRadius = UDim.new(0, 4)
predictionCorner.Parent = predictionFrame

local predictionLabel = Instance.new("TextLabel")
predictionLabel.Name = "PredictionLabel"
predictionLabel.Size = UDim2.new(1, 0, 1, 0)
predictionLabel.Text = "PREDICTION: 0ms"
predictionLabel.TextColor3 = Color3.new(1, 1, 1)
predictionLabel.TextSize = 12
predictionLabel.Font = Enum.Font.GothamBold
predictionLabel.BackgroundTransparency = 1
predictionLabel.Parent = predictionFrame
predictionFrame.Parent = screenGui

mainFrame.Parent = screenGui

-- Lock system variables
local lockedPlayer = nil
local isLocked = false
local lockRange = 2000
local smoothness = 0.3
local jumpOffset = 1.5
local predictionHistory = {}
local maxPredictionHistory = 10

-- Velocity prediction function
local function predictPosition(targetRoot, dt)
    if not targetRoot then return nil end
    
    -- Get current velocity
    local velocity = targetRoot.AssemblyLinearVelocity
    
    -- Simple prediction: position + velocity * time
    return targetRoot.Position + (velocity * dt)
end

-- Enhanced target selection with prediction
local function findBestTarget()
    if not player.Character then return nil end
    local root = player.Character:FindFirstChild("HumanoidRootPart")
    if not root then return nil end
    
    local bestTarget = nil
    local bestScore = -math.huge
    
    for _, otherPlayer in ipairs(Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character then
            local targetRoot = otherPlayer.Character:FindFirstChild("HumanoidRootPart")
            local humanoid = otherPlayer.Character:FindFirstChildOfClass("Humanoid")
            
            if targetRoot and humanoid then
                local distance = (targetRoot.Position - root.Position).Magnitude
                if distance <= lockRange then
                    -- Calculate direction to target
                    local direction = (targetRoot.Position - root.Position).Unit
                    
                    -- Calculate angle from camera view
                    local viewAngle = math.deg(math.acos(camera.CFrame.LookVector:Dot(direction)))
                    
                    -- Calculate score (prioritize targets in center of screen and closer)
                    local score = (1 - viewAngle/180) * 0.6 + (1 - distance/lockRange) * 0.4
                    
                    -- Slight bonus if target is moving
                    if targetRoot.AssemblyLinearVelocity.Magnitude > 5 then
                        score = score * 1.1
                    end
                    
                    if score > bestScore then
                        bestScore = score
                        bestTarget = otherPlayer
                    end
                end
            end
        end
    end
    
    return bestTarget
end

-- Smooth camera lock with prediction and jump offset
local function updateCamLock(dt)
    if not isLocked or not lockedPlayer or not lockedPlayer.Character then return end
    
    local targetRoot = lockedPlayer.Character:FindFirstChild("HumanoidRootPart")
    local humanoid = lockedPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not targetRoot or not humanoid then return end
    
    -- Calculate predicted position
    local predictedPosition = predictPosition(targetRoot, dt)
    if not predictedPosition then return end
    
    -- Apply jump offset (move camera up when target jumps)
    local verticalOffset = 0
    if humanoid.Jump then
        verticalOffset = jumpOffset
    end
    
    predictedPosition = predictedPosition + Vector3.new(0, verticalOffset, 0)
    
    -- Smooth camera movement
    local currentCFrame = camera.CFrame
    local currentPosition = currentCFrame.Position
    local desiredLookVector = (predictedPosition - currentPosition).Unit
    
    -- Use spherical linear interpolation for smoother rotation
    local newLookVector = currentCFrame.LookVector:Lerp(desiredLookVector, smoothness)
    
    -- Update camera
    camera.CFrame = CFrame.new(currentPosition, currentPosition + newLookVector)
    
    -- Update prediction display
    table.insert(predictionHistory, dt)
    if #predictionHistory > maxPredictionHistory then
        table.remove(predictionHistory, 1)
    end
    
    local avgPrediction = 0
    for _, val in ipairs(predictionHistory) do
        avgPrediction = avgPrediction + val
    end
    avgPrediction = avgPrediction / #predictionHistory
    
    predictionLabel.Text = string.format("PREDICTION: %.1fms", avgPrediction * 1000)
end

-- Update UI function
local function updateUI()
    if isLocked and lockedPlayer then
        lockButton.Text = "LOCK: "..string.sub(lockedPlayer.Name, 1, 12)
        lockButton.BackgroundColor3 = Color3.fromRGB(180, 40, 40)
        lockStroke.Color = Color3.fromRGB(255, 80, 80)
        
        -- Pulse effect
        local tweenInfo = TweenInfo.new(0.8, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
        local tween = TweenService:Create(lockStroke, tweenInfo, {Thickness = 2})
        tween:Play()
    else
        lockButton.Text = "LOCK: OFF"
        lockButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        lockStroke.Color = Color3.fromRGB(100, 100, 100)
        lockStroke.Thickness = 1
    end
end

-- Slider interaction
local function setupSlider(slider, fill, label, minValue, maxValue, currentValue, callback)
    local dragging = false
    
    slider.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
        end
    end)
    
    slider.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
            local mousePos = UserInputService:GetMouseLocation()
            local sliderPos = slider.AbsolutePosition
            local sliderSize = slider.AbsoluteSize
            
            local relativeX = math.clamp((mousePos.X - sliderPos.X) / sliderSize.X, 0, 1)
            local value = minValue + (maxValue - minValue) * relativeX
            
            fill.Size = UDim2.new(relativeX, 0, 1, 0)
            callback(value)
            
            if label then
                label.Text = string.format("%s: %.1f", label.Text:match("([^:]+)"), value)
            end
        end
    end)
    
    -- Initialize
    local initialRatio = (currentValue - minValue) / (maxValue - minValue)
    fill.Size = UDim2.new(initialRatio, 0, 1, 0)
end

-- Setup sliders
setupSlider(
    smoothnessSlider,
    smoothnessFill,
    smoothnessLabel,
    0.1, -- min
    0.9, -- max
    smoothness, -- current
    function(value) smoothness = value end
)

setupSlider(
    jumpOffsetSlider,
    jumpOffsetFill,
    jumpOffsetLabel,
    0.5, -- min
    3.0, -- max
    jumpOffset, -- current
    function(value) jumpOffset = value end
)

-- Toggle lock
lockButton.MouseButton1Click:Connect(function()
    isLocked = not isLocked
    
    if isLocked then
        lockedPlayer = findBestTarget()
        if lockedPlayer then
            predictionHistory = {}
            RunService:BindToRenderStep("CamLockUpdate", Enum.RenderPriority.Camera.Value + 1, updateCamLock)
        else
            isLocked = false
        end
    else
        RunService:UnbindFromRenderStep("CamLockUpdate")
    end
    
    updateUI()
end)

-- Cleanup when character changes
player.CharacterAdded:Connect(function()
    if isLocked then
        lockedPlayer = findBestTarget()
        updateUI()
    end
end)

player.CharacterRemoving:Connect(function()
    if isLocked then
        RunService:UnbindFromRenderStep("CamLockUpdate")
    end
end)

-- Initial UI update
updateUI()
