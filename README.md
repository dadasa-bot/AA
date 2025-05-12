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
btn.Font = Enum.Font.GothamBold
btn.Parent = screenGui
btn.Active = true
btn.Draggable = true
    
local btncorner = Instance.new("UICorner")
btncorner.CornerRadius = UDim.new(0, 10)
btncorner.Parent = btn

btn.MouseButton1Click:Connect(function()
    if not hasLoaded then
        hasLoaded = true
        -- Replace this with the actual safe code you want to run
        loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/anti-afk%20via%20autofocus.txt"))()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Success";
            Text = "Script loaded!";
            Duration = 3;
        })
    else
        game.StarterGui:SetCore("SendNotification", {
            Title = "Notice";
            Text = "Already loaded 😐";
            Duration = 3;
        })
    end
end)
