local rs = game:GetService("RunService")
local plrs = game:GetService("Players")
local cg = game:GetService("CoreGui")
local localPlayer = plrs.LocalPlayer
local mouse = localPlayer:GetMouse()

-- Create GUI
local screenGui = Instance.new("ScreenGui", cg)
screenGui.Name = "Google"

local btn = Instance.new("TextButton", screenGui)
btn.Name = "btn"
btn.Text = "EngineApi"
btn.Size = UDim2.new(0, 120, 0, 40)
btn.Position = UDim2.new(0, 30, 0, 30)
btn.BackgroundColor3 = Color3.fromRGB(140, 40, 40)
btn.TextColor3 = Color3.fromRGB(0, 0, 0)
btn.TextScaled = true 
btn.Font = Enum.Font.Arcade
btn.Active = true
btn.Draggable = true

local ui = Instance.new("UICorner", btn)
ui.CornerRadius = UDim.new(0, 10)

-- Button Click Function
btn.MouseButton1Click:Connect(function()
    rs.Heartbeat:Connect(function()
        local closestPlayer = nil
        local shortestDistance = math.huge

        for _, player in pairs(plrs:GetPlayers()) do
            if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local dist = (player.Character.HumanoidRootPart.Position - localPlayer.Character.HumanoidRootPart.Position).Magnitude
                if dist < shortestDistance then
                    shortestDistance = dist
                    closestPlayer = player
                end
            end
        end

        if closestPlayer then
            btn.Text = "Detected: " .. closestPlayer.Name
            warn("Player Detected:", closestPlayer.Name)
        else
            btn.Text = "No Players Detected"
            warn("No Players Detected")
        end

        task.wait(1)
    end)
end)
