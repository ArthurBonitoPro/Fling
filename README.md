-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

-- Variáveis
local player = Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
local frame = Instance.new("Frame")
local textLabel = Instance.new("TextLabel")

-- Configuração da GUI
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false

-- Quadrado principal
frame.Parent = screenGui
frame.Size = UDim2.new(0, 200, 0, 80) -- Tamanho do quadrado
frame.Position = UDim2.new(0.5, -100, 0, 10) -- Posição inicial (topo central)
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
frame.BorderSizePixel = 0
frame.ClipsDescendants = true

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.Parent = frame
corner.CornerRadius = UDim.new(0, 12)

-- Texto das coordenadas
textLabel.Parent = frame
textLabel.Size = UDim2.new(0.9, 0, 0.8, 0)
textLabel.Position = UDim2.new(0.05, 0, 0.1, 0)
textLabel.BackgroundTransparency = 1
textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
textLabel.TextSize = 16
textLabel.Font = Enum.Font.SourceSansBold
textLabel.Text = "Coordenadas:\nX: 0\nY: 0\nZ: 0"
textLabel.TextXAlignment = Enum.TextXAlignment.Left
textLabel.TextYAlignment = Enum.TextYAlignment.Top

-- Função para atualizar as coordenadas
local function updateCoordinates()
    local character = player.Character
    if character then
        local humanoidRootPart = character:FindFirstChild("HumanoidRootPart")
        if humanoidRootPart then
            local position = humanoidRootPart.Position
            textLabel.Text = string.format("Coordenadas:\nX: %.1f\nY: %.1f\nZ: %.1f", position.X, position.Y, position.Z)
        end
    end
end

-- Função para arrastar o quadrado
local dragging = false
local dragStartPosition = Vector2.new(0, 0)
local frameStartPosition = Vector2.new(0, 0)

local function startDrag(input)
    dragging = true
    dragStartPosition = Vector2.new(input.Position.X, input.Position.Y)
    frameStartPosition = Vector2.new(frame.Position.X.Offset, frame.Position.Y.Offset)
end

local function stopDrag()
    dragging = false
end

local function updateDrag(input)
    if dragging then
        local delta = Vector2.new(input.Position.X, input.Position.Y) - dragStartPosition
        frame.Position = UDim2.new(0, frameStartPosition.X + delta.X, 0, frameStartPosition.Y + delta.Y)
    end
end

-- Conecta os eventos de arrastar
frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        startDrag(input)
    end
end)

frame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        stopDrag()
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
        updateDrag(input)
    end
end)

-- Conecta a atualização das coordenadas ao loop do jogo
RunService.Heartbeat:Connect(updateCoordinates)
