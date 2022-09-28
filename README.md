local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("cripple hub", "Synapse")
-- MAIN
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Walkspeed")
local MainSection = Main:NewSection("Gravity")

MainSection:NewSlider("Walkspeed", "Pick Your Walkspeed", 500, 0, function(s) -- 500 (MaxValue) | 0 (MinValue)
    game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = s
end)

MainSection:NewSlider("Gravity", "Change the gravity", 196.2, 10, function(s) -- 500 (MaxValue) | 0 (MinValue)
    workspace.Gravity = s
end)

MainSection:NewButton("Inf Jump", "Unreversable", function()
    -- This should be in StarterCharacterScripts
local UserInputService = game:GetService("UserInputService")
local player = game:GetService("Players").LocalPlayer
local character = player.Character
local humanoid = character:WaitForChild("Humanoid")

local MAX_JUMPS = 1000000
local TIME_BETWEEN_JUMPS = 0.2
local numJumps = 0
local canJumpAgain = false

local function onStateChanged(oldState, newState)
	if Enum.HumanoidStateType.Landed == newState then
		numJumps = 0
		canJumpAgain = false
	elseif Enum.HumanoidStateType.Freefall == newState then
		wait(TIME_BETWEEN_JUMPS)
		canJumpAgain = true
	elseif Enum.HumanoidStateType.Jumping == newState then
		canJumpAgain = false
		numJumps += 1
	end

end

local function onJumpRequest()
	if canJumpAgain and numJumps < MAX_JUMPS then
		humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end

humanoid.StateChanged:Connect(onStateChanged)
UserInputService.JumpRequest:Connect(onJumpRequest)
end)
