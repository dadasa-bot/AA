local screenGui = Instance.new("ScreenGui")
screenGui.Name = "screenGui"
screenGui.Parent = game.CoreGui

local btn = Instance.new("TextButton")
btn.Name = "textbutton"
btn.Text = ("We are niggers")
btn.Size = UDim2.new(0, 100, 0, 100)
btn.Position = UDim2.new(0, 30, 0, 30)
btn.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
btn.TextColor3 = Color3.fromRGB(255, 255, 255)
btn.TextScaled = true
btn.Parent = screenGui
btn.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/anti-afk%20via%20autofocus.txt"))()
end)
    
local btncorner = Instance.new("UICorner")
btncorner.CornerRadius = UDim.new(0, 10)
btncorner.Parent = btn
