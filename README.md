local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
   Name = "Rayfield Example Window",
   Icon = 0,
   LoadingTitle = "Rayfield Interface Suite",
   LoadingSubtitle = "by Sirius",
   Theme = "Light",

   DisableRayfieldPrompts = false,
   DisableBuildWarnings = false,

   ConfigurationSaving = {
      Enabled = true,
      FolderName = nil,
      FileName = "Big Hub"
   },

   Discord = {
      Enabled = true,
      Invite = "MzhfAsME",
      RememberJoins = true
   },

   KeySystem = true,
   KeySettings = {
      Title = "Key",
      Subtitle = "Key System",
      Note = "key is Hello",
      FileName = "Key",
      SaveKey = true,
      GrabKeyFromSite = false,
      Key = {"Hello"}
   }
})

local PlayerTab = Window:CreateTab("Player", 0)

PlayerTab:CreateSlider({
   Name = "WalkSpeed",
   Range = {16, 100},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 100,
   Flag = "WalkSpeedSlider",
   Callback = function(Value)
      local player = game.Players.LocalPlayer
      if not player.Character or not player.Character:FindFirstChild("Humanoid") then
         player.CharacterAdded:Wait()
         repeat task.wait() until player.Character:FindFirstChild("Humanoid")
      end
      player.Character.Humanoid.WalkSpeed = Value
   end,
})
