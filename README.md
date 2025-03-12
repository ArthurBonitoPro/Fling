-- Serviços
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

-- Variáveis
local player = Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local textBox = Instance.new("TextBox")
local addButton = Instance.new("TextButton")

-- Configuração da GUI
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- Quadrado principal
frame.Parent = screenGui
frame.Size = UDim2.new(0, 200, 0, 150) -- Tamanho do quadrado
frame.Position = UDim2.new(0.5, -100, 0.5, -75) -- Centralizado
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.ClipsDescendants = true

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.Parent = frame
corner.CornerRadius = UDim.new(0, 12)

-- Campo de texto para digitar a quantidade de vida
textBox.Parent = frame
textBox.Size = UDim2.new(0.8, 0, 0.3, 0)
textBox.Position = UDim2.new(0.1, 0, 0.1, 0)
textBox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
textBox.TextColor3 = Color3.fromRGB(255, 255, 255)
textBox.TextSize = 16
textBox.Font = Enum.Font.SourceSansBold
textBox.PlaceholderText = "Digite a quantidade de vida"
textBox.ClearTextOnFocus = false

-- Cantos arredondados para o campo de texto
local textBoxCorner = Instance.new("UICorner")
textBoxCorner.Parent = textBox
textBoxCorner.CornerRadius = UDim.new(0, 8)

-- Botão "Adicionar"
addButton.Parent = frame
addButton.Size = UDim2.new(0.8, 0, 0.3, 0)
addButton.Position = UDim2.new(0.1, 0, 0.5, 0)
addButton.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
addButton.TextColor3 = Color3.fromRGB(255, 255, 255)
addButton.TextSize = 16
addButton.Font = Enum.Font.SourceSansBold
addButton.Text = "Adicionar"

-- Cantos arredondados para o botão
local buttonCorner = Instance.new("UICorner")
buttonCorner.Parent = addButton
buttonCorner.CornerRadius = UDim.new(0, 8)

-- Função para adicionar vida
local function adicionarVida()
    local quantidade = tonumber(textBox.Text) -- Converte o texto para número
    if quantidade and quantidade > 0 then
        local character = player.Character
        if character then
            local humanoid = character:FindFirstChild("Humanoid")
            if humanoid then
                humanoid.Health = humanoid.Health + quantidade
                print("Vida adicionada: " .. quantidade)
            else
                print("Humanoid não encontrado!")
            end
        else
            print("Personagem não encontrado!")
        end
    else
        print("Digite um valor válido!")
    end
end

-- Conecta o botão à função
addButton.MouseButton1Click:Connect(adicionarVida)
