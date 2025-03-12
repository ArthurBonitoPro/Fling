-- Configurações SCRIPT

-- Gui to Lua (VIP VERSION)
-- Version: 6.9

-- Instances:

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Frame_2 = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")
local TextButton = Instance.new("TextButton")

--Properties:

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(34, 34, 34)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.388539821, 0, 0.427821517, 0)
Frame.Size = UDim2.new(0, 200, 0, 120)

Frame_2.Parent = Frame
Frame_2.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame_2.BorderSizePixel = 0
Frame_2.Size = UDim2.new(0, 200, 0, 30)

TextLabel.Parent = Frame_2
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextLabel.BorderSizePixel = 0
TextLabel.Position = UDim2.new(0.1, 0, 0, 0)
TextLabel.Size = UDim2.new(0, 180, 0, 30)
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.Text = "AUTO PARRY"
TextLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
TextLabel.TextSize = 20.000

TextButton.Parent = Frame
TextButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextButton.BorderSizePixel = 0
TextButton.Position = UDim2.new(0.1, 0, 0.4, 0)
TextButton.Size = UDim2.new(0, 160, 0, 40)
TextButton.Font = Enum.Font.SourceSansBold
TextButton.Text = "ATIVAR"
TextButton.TextColor3 = Color3.fromRGB(0, 0, 0)
TextButton.TextSize = 20.000

-- Scripts:

local function autoParryScript()
    local ball = workspace:FindFirstChild("Ball") -- Substitua "Ball" pelo nome correto da bola no jogo
    local player = game.Players.LocalPlayer
    local character = player.Character
    local rootPart = character and character:FindFirstChild("HumanoidRootPart")
    local autoParryEnabled = false

    local function checkDistance()
        if autoParryEnabled and ball and rootPart then
            local distance = (ball.Position - rootPart.Position).Magnitude
            if distance < 10 then -- Defina a distância de ativação do parry
                -- Simula o clique do parry (substitua pela função correta do jogo)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, false)
                task.wait(0.1)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, false)
            end
        end
    end

    TextButton.MouseButton1Click:Connect(function()
        autoParryEnabled = not autoParryEnabled
        TextButton.Text = autoParryEnabled and "DESATIVAR" or "ATIVAR"
        TextButton.TextColor3 = autoParryEnabled and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 0, 0)
    end)

    while true do
        checkDistance()
        task.wait() -- Ajuste o tempo de verificação conforme necessário
    end
end

coroutine.wrap(autoParryScript)()
