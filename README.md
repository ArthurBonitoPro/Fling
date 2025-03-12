-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Variáveis
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

-- Lista de coordenadas (X, Y, Z)
local coordenadas = {
    Vector3.new(143.5, 9285.0, 74.9),   -- Coordenada 1
    Vector3.new(144.8, 5657.0, 74.1),   -- Coordenada 2
    Vector3.new(142.8, 4047.2, 63.1),   -- Coordenada 3
    Vector3.new(228.5, 2013.8, 265.2),  -- Coordenada 4
    Vector3.new(91.8, 766.0, -127.3)    -- Coordenada 5
}

-- Função para teleportar o jogador
local function teleportarPara(coordenada)
    humanoidRootPart.CFrame = CFrame.new(coordenada)
    print("Teleportado para: " .. tostring(coordenada))
end

-- Loop para teleportar para cada coordenada com intervalo de 5 segundos
for i, coordenada in ipairs(coordenadas) do
    teleportarPara(coordenada)
    wait(5) -- Aguarda 5 segundos antes de ir para a próxima coordenada
end

print("Teleporte concluído!")
