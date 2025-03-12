-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- Variáveis
local player = Players.LocalPlayer
local flingEnabled = false
local flingForce = 10000 -- Força do fling

-- Cria a GUI
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- Quadrado principal
local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.new(0, 120, 0, 80) -- Tamanho pequeno para mobile
frame.Position = UDim2.new(0.5, -60, 0.5, -40) -- Centralizado
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.ClipsDescendants = true

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.Parent = frame
corner.CornerRadius = UDim.new(0, 12)

-- Texto "Script"
local scriptLabel = Instance.new("TextLabel")
scriptLabel.Parent = frame
scriptLabel.Text = "Script"
scriptLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
scriptLabel.TextSize = 18
scriptLabel.Font = Enum.Font.SourceSansBold
scriptLabel.BackgroundTransparency = 1
scriptLabel.Position = UDim2.new(0.1, 0, 0.1, 0)
scriptLabel.Size = UDim2.new(0.8, 0, 0.3, 0)

-- Botão de ativar/desativar
local button = Instance.new("TextButton")
button.Parent = frame
button.Text = "Ativar"
button.TextColor3 = Color3.fromRGB(255, 255, 255)
button.TextSize = 16
button.Font = Enum.Font.SourceSansBold
button.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
button.BorderSizePixel = 0
button.Position = UDim2.new(0.1, 0, 0.5, 0)
button.Size = UDim2.new(0.8, 0, 0.3, 0)

-- Cantos arredondados para o botão
local buttonCorner = Instance.new("UICorner")
buttonCorner.Parent = button
buttonCorner.CornerRadius = UDim.new(0, 8)

-- Função de Fling
local function flingPlayer(target)
    local character = player.Character
    local targetCharacter = target.Character

    if character and targetCharacter then
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        local targetRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")

        if humanoidRootPart and targetRootPart then
            local direction = (targetRootPart.Position - humanoidRootPart.Position).Unit
            targetRootPart.Velocity = direction * flingForce
        end
    end
end

-- Verifica toques no jogador
local function onTouch(otherPart)
    if flingEnabled then
        local touchedPlayer = Players:GetPlayerFromCharacter(otherPart.Parent)
        if touchedPlayer and touchedPlayer ~= player then
            flingPlayer(touchedPlayer)
        end
    end
end

-- Conecta o evento de toque
local function connectTouchEvent()
    local character = player.Character
    if character then
        for _, part in pairs(character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Touched:Connect(onTouch)
            end
        end
    end
end

-- Atualiza o evento de toque quando o jogador respawna
player.CharacterAdded:Connect(connectTouchEvent)
if player.Character then
    connectTouchEvent()
end

-- Alterna o estado do fling
button.MouseButton1Click:Connect(function()
    flingEnabled = not flingEnabled
    button.Text = flingEnabled and "Ativado" or "Ativar"
end)
