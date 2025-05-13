-- Load Rayfield UI Library (working as of 2025)
local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/REDzHUB/RayfieldLibrary/main/Rayfield.lua"))()

-- Create the UI Window
local Window = Rayfield:CreateWindow({
   Name = "DevTools Hub",
   LoadingTitle = "DevTools",
   LoadingSubtitle = "by YourNameHere",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DevTools",
      FileName = "DevToolsConfig"
   },
   Discord = {
      Enabled = false,
      Invite = "",
      RememberJoins = false
   },
   KeySystem = false,
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
local MainTab = Window:CreateTab("Main", 4483362458)

-- Add UI elements
MainTab:CreateButton({
   Name = "Print Hello",
   Callback = function()
      print("Hello, developer!")
   end,
})

MainTab:CreateToggle({
   Name = "Enable Feature",
   CurrentValue = false,
   Callback = function(Value)
      print("Feature toggled:", Value)
   end,
})

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

MainTab:CreateInput({
   Name = "Enter Text",
   PlaceholderText = "Type something...",
   RemoveTextAfterFocusLost = false,
   Callback = function(Text)
      print("You typed:", Text)
   end,
})
