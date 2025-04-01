local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Lighting = game:GetService("Lighting")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local flying = false
local speed = 50  -- Velocidade do voo

-- Referência aos botões da GUI
local screenGui = script.Parent
local flyButton = screenGui:FindFirstChild("FlyButton")
local nightButton = screenGui:FindFirstChild("NightButton")
local knifeButton = screenGui:FindFirstChild("KnifeButton")

-- Função para voar
local function fly()
    if not flying then
        flying = true
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, speed, 0)
        bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        bodyVelocity.Parent = character.PrimaryPart

        while flying do
            task.wait()
            bodyVelocity.Velocity = character.PrimaryPart.CFrame.LookVector * speed
        end
    else
        flying = false
        if character.PrimaryPart then
            for _, obj in pairs(character.PrimaryPart:GetChildren()) do
                if obj:IsA("BodyVelocity") then
                    obj:Destroy()
                end
            end
        end
    end
end

-- Função para alternar entre dia e noite
local function toggleNight()
    if Lighting.ClockTime >= 18 or Lighting.ClockTime < 6 then
        Lighting.ClockTime = 12  -- Define para meio-dia
    else
        Lighting.ClockTime = 24  -- Define para meia-noite
    end
end

-- Função para dar uma faca
local function giveKnife()
    local backpack = player:FindFirstChild("Backpack")
    local knife = ReplicatedStorage:FindFirstChild("Knife")
    
    if knife and backpack then
        local clonedKnife = knife:Clone()
        clonedKnife.Parent = backpack  -- Adiciona a faca ao inventário do jogador
    end
end

-- Conectar os botões às funções
flyButton.MouseButton1Click:Connect(fly)
nightButton.MouseButton1Click:Connect(toggleNight)
knifeButton.MouseButton1Click:Connect(giveKnife)
