-- Load Rayfield (Delta-Compatible)
local Rayfield = loadstring(game:HttpGet("https://raw.githubusercontent.com/cheesynob39/Rayfield-ReVanced/main/Source.lua"))()

-- Create the main UI
local Window = Rayfield:CreateWindow({
   Name = "Mobile WalkSpeed GUI",
   LoadingTitle = "Loading...",
   LoadingSubtitle = "Delta Android Edition",
   Theme = "Light",

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false,

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil,
      FileName = "WalkSpeedDelta"
   },

   Discord = {
      Enabled = false -- Discord doesn't open on mobile executors anyway
   },

   KeySystem = false
})

-- Player Tab
local PlayerTab = Window:CreateTab("Player", 0)

-- WalkSpeed Slider
PlayerTab:CreateSlider({
   Name = "WalkSpeed",
   Range = {16, 100},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Callback = function(Value)
      local lp = game.Players.LocalPlayer
      local char = lp.Character or lp.CharacterAdded:Wait()
      local humanoid = char:WaitForChild("Humanoid", 5)
      if humanoid then
         humanoid.WalkSpeed = Value
      end
   end
})
