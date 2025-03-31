-- LocalScript para controle de voo e GUI
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local flying = false
local flyingSpeed = 50  -- Velocidade inicial do voo

-- Criando o GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Botão para ativar/desativar voo
local toggleButton = Instance.new("TextButton")
toggleButton.Size = UDim2.new(0, 200, 0, 50)
toggleButton.Position = UDim2.new(0, 0, 0, 50)
toggleButton.Text = "Ativar Voo"
toggleButton.Parent = screenGui

-- Botão para aumentar a velocidade do voo
local speedUpButton = Instance.new("TextButton")
speedUpButton.Size = UDim2.new(0, 200, 0, 50)
speedUpButton.Position = UDim2.new(0, 0, 0, 110)
speedUpButton.Text = "Aumentar Velocidade"
speedUpButton.Parent = screenGui

-- Botão para diminuir a velocidade do voo
local speedDownButton = Instance.new("TextButton")
speedDownButton.Size = UDim2.new(0, 200, 0, 50)
speedDownButton.Position = UDim2.new(0, 0, 0, 170)
speedDownButton.Text = "Diminuir Velocidade"
speedDownButton.Parent = screenGui

-- Criando a "Parte de voo"
local flyingPart = Instance.new("Part")
flyingPart.Size = Vector3.new(5, 1, 5)
flyingPart.Position = player.Character.HumanoidRootPart.Position + Vector3.new(0, 5, 0)
flyingPart.Anchored = true
flyingPart.CanCollide = false
flyingPart.Parent = workspace

-- Função para alternar voo
local function toggleFly()
    flying = not flying
    if flying then
        toggleButton.Text = "Desativar Voo"
    else
        toggleButton.Text = "Ativar Voo"
    end
end

-- Função para aumentar a velocidade do voo
local function increaseSpeed()
    flyingSpeed = flyingSpeed + 10  -- Aumenta a velocidade em 10 unidades
    print("Velocidade aumentada: " .. flyingSpeed)
end

-- Função para diminuir a velocidade do voo
local function decreaseSpeed()
    if flyingSpeed > 10 then
        flyingSpeed = flyingSpeed - 10  -- Diminui a velocidade em 10 unidades
        print("Velocidade diminuída: " .. flyingSpeed)
    end
end

-- Conectar funções aos botões da GUI
toggleButton.MouseButton1Click:Connect(toggleFly)
speedUpButton.MouseButton1Click:Connect(increaseSpeed)
speedDownButton.MouseButton1Click:Connect(decreaseSpeed)

-- Função de controle de voo
game:GetService("RunService").Heartbeat:Connect(function()
    if flying and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local hrp = player.Character.HumanoidRootPart
        hrp.Velocity = Vector3.new(0, flyingSpeed, 0)
        
        -- Para mover o personagem pelo mouse
        local direction = (mouse.Hit.p - hrp.Position).unit
        hrp.CFrame = CFrame.new(hrp.Position + direction * flyingSpeed)
    end
end)
