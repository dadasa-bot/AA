-- Load Rayfield UI Library
local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Rayfield/main/source"))()

-- Create the UI Window
local Window = Rayfield:CreateWindow({
   Name = "DevTools Hub",
   LoadingTitle = "DevTools",
   LoadingSubtitle = "by YourNameHere",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DevTools", -- Change this to your desired folder name
      FileName = "DevToolsConfig"
   },
   Discord = {
      Enabled = false,
      Invite = "", -- Your Discord server invite (optional)
      RememberJoins = false
   },
   KeySystem = false, -- Change to true if you want a key system
   KeySettings = {
      Title = "Dev Key",
      Subtitle = "Key System",
      Note = "Ask the owner for the key.",
      FileName = "DevKey",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = "ABC123"
   }
})

-- Create a tab
local MainTab = Window:CreateTab("Main", 4483362458) -- Icon is optional

-- Add a button
MainTab:CreateButton({
   Name = "Print Hello",
   Callback = function()
      print("Hello, developer!")
   end,
})

-- Add a toggle
MainTab:CreateToggle({
   Name = "Enable Feature",
   CurrentValue = false,
   Callback = function(Value)
      print("Feature toggled:", Value)
   end,
})

-- Add a slider
MainTab:CreateSlider({
   Name = "Speed",
   Range = {0, 100},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 50,
   Callback = function(Value)
      print("Speed set to:", Value)
   end,
})

-- Add an input
MainTab:CreateInput({
   Name = "Enter Text",
   PlaceholderText = "Type something...",
   RemoveTextAfterFocusLost = false,
   Callback = function(Text)
      print("You typed:", Text)
   end,
})
