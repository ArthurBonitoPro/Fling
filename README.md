-- Obtém o jogador
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Cria a ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = playerGui

-- Cria o Frame principal (menu)
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 250) -- Tamanho médio
frame.Position = UDim2.new(0.5, -100, 0.5, -125) -- Centralizado
frame.BackgroundColor3 = Color3.new(0, 0.4, 1) -- Cor azul
frame.Active = true -- Permite que o Frame seja clicado
frame.Draggable = true -- Permite que o Frame seja arrastado
frame.Parent = screenGui

-- Adiciona UICorner para bordas arredondadas
local uiCorner = Instance.new("UICorner")
uiCorner.CornerRadius = UDim.new(0, 10) -- Bordas arredondadas
uiCorner.Parent = frame

-- Adiciona créditos (Arthur)
local credits = Instance.new("TextLabel")
credits.Text = "Créditos: Arthur"
credits.Size = UDim2.new(0, 180, 0, 20)
credits.Position = UDim2.new(0.1, 0, 0.05, 0)
credits.TextColor3 = Color3.new(1, 1, 1) -- Texto branco
credits.BackgroundTransparency = 1 -- Fundo transparente
credits.Parent = frame

-- Função para criar botões
local function createButton(text, positionY, callback)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, 180, 0, 40)
    button.Position = UDim2.new(0.1, 0, positionY, 0)
    button.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2) -- Cor do botão
    button.TextColor3 = Color3.new(1, 1, 1) -- Texto branco
    button.Font = Enum.Font.SourceSansBold -- Fonte em negrito
    button.Parent = frame

    -- Adiciona UICorner ao botão
    local buttonCorner = Instance.new("UICorner")
    buttonCorner.CornerRadius = UDim.new(0, 5) -- Bordas arredondadas
    buttonCorner.Parent = button

    -- Conecta o clique do botão à função de callback
    button.MouseButton1Click:Connect(callback)
end

-- Comando: Atravessar Parede
createButton("Atravessar Parede", 0.2, function()
    local character = player.Character or player.CharacterAdded:Wait()
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = false -- Desativa colisão
        end
    end
    print("Atravessar Parede ativado!")
end)

-- Comando: Definir Velocidade
createButton("Definir Velocidade", 0.4, function()
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChild("Humanoid")
    if humanoid then
        humanoid.WalkSpeed = 50 -- Define a velocidade para 50
    end
    print("Velocidade definida para 50!")
end)

-- Comando: FLING
createButton("FLING", 0.6, function()
    local character = player.Character or player.CharacterAdded:Wait()
    local rootPart = character:FindFirstChild("HumanoidRootPart")
    if rootPart then
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = Vector3.new(0, 100, 0) -- Lança o jogador para cima
        bodyVelocity.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        bodyVelocity.Parent = rootPart
        wait(0.5)
        bodyVelocity:Destroy()
    end
    print("FLING ativado!")
end)
