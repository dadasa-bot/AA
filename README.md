-- Rayfield Revanced (working as of 2025)
local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/cheesynob39/Rayfield-ReVanced/main/Source.lua"))()

-- Create the window
local Window = Rayfield:CreateWindow({
   Name = "DevTools Revanced",
   LoadingTitle = "Rayfield ReVanced",
   LoadingSubtitle = "Custom GUI",
   ConfigurationSaving = {
      Enabled = true,
      FolderName = "DevToolsRev",
      FileName = "RevConfig"
   },
   Discord = {
      Enabled = false
   },
   KeySystem = false
})

-- Create a tab
local Tab = Window:CreateTab("Main", 4483362458)

-- Add a button
Tab:CreateButton({
   Name = "Print Something",
   Callback = function()
      print("Rayfield ReVanced is working!")
   end,
})

-- Add other UI elements as needed...
