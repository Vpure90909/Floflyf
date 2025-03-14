local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local flying = false
local speed = 50
local bodyVelocity = Instance.new("BodyVelocity")
local bodyGyro = Instance.new("BodyGyro")

local function startFlying()
    flying = true
    bodyVelocity.MaxForce = Vector3.new(50000, 50000, 50000)
    bodyVelocity.Velocity = Vector3.new(0, speed, 0)
    bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")

    bodyGyro.MaxTorque = Vector3.new(50000, 50000, 50000)
    bodyGyro.CFrame = character.HumanoidRootPart.CFrame
    bodyGyro.Parent = character:WaitForChild("HumanoidRootPart")
end

local function stopFlying()
    flying = false
    bodyVelocity:Destroy()
    bodyGyro:Destroy()
end

game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.UserInputType == Enum.UserInputType.Touch then
        if flying then
            stopFlying()
        else
            startFlying()
        end
    end
end)
