-- Services
local rs = game:GetService("RunService")
local plrs = game:GetService("Players")
local cg = game:GetService("CoreGui")
local lp = plrs.LocalPlayer
local mouse = lp:GetMouse()

-- GUI setup
local gui = Instance.new("ScreenGui", cg)
gui.Name = "MouseLockGUI"

local btn = Instance.new("TextButton", gui)
btn.Size = UDim2.new(0, 200, 0, 40)
btn.Position = UDim2.new(0, 30, 0, 30)
btn.Text = "Start Mouse Targeting"
btn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
btn.TextColor3 = Color3.new(1, 1, 1)
btn.TextScaled = true
btn.Font = Enum.Font.GothamBold
btn.Draggable = true
btn.Active = true

local corner = Instance.new("UICorner", btn)
corner.CornerRadius = UDim.new(0, 10)

-- Toggle logic
local detecting = false
local conn

btn.MouseButton1Click:Connect(function()
    detecting = not detecting

    if detecting then
        btn.Text = "Mouse Targeting On"
        print("[MouseTarget] Started")

        conn = rs.RenderStepped:Connect(function()
            local target = mouse.Target
            if target then
                local model = target:FindFirstAncestorOfClass("Model")
                local player = plrs:GetPlayerFromCharacter(model)

                if player and player ~= lp and player.Character and player.Character:FindFirstChild("HumanoidRootPart") and lp.Character and lp.Character:FindFirstChild("HumanoidRootPart") then
                    local dist = (lp.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
                    btn.Text = "Locked: " .. player.Name .. " [" .. math.floor(dist) .. " studs]"
                    print("[MouseTarget] Target:", player.Name)

                    -- 🔧 Optional Camlock
                    -- workspace.CurrentCamera.CFrame = CFrame.new(workspace.CurrentCamera.CFrame.Position, player.Character.HumanoidRootPart.Position)
                else
                    btn.Text = "Not aiming at player"
                end
            else
                btn.Text = "No target under mouse"
            end
        end)
    else
        btn.Text = "Start Mouse Targeting"
        print("[MouseTarget] Stopped")
        if conn then conn:Disconnect() end
    end
end)
