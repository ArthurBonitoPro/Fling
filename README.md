-- Configurações SCRIPT

-- Gui to Lua (VIP VERSION)
-- Version: 6.9

-- Instances:

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local ScrollingFrame = Instance.new("ScrollingFrame")
local ToolTemplate = Instance.new("TextButton")
local SelectedToolFrame = Instance.new("Frame")
local SelectedToolLabel = Instance.new("TextLabel")
local PickButton = Instance.new("TextButton")

--Properties:

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.3, 0, 0.3, 0)
Frame.Size = UDim2.new(0, 300, 0, 400)
Frame.Active = true
Frame.Draggable = true

UICorner.Parent = Frame
UICorner.CornerRadius = UDim.new(0, 10)

ScrollingFrame.Parent = Frame
ScrollingFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
ScrollingFrame.BorderSizePixel = 0
ScrollingFrame.Position = UDim2.new(0.05, 0, 0.05, 0)
ScrollingFrame.Size = UDim2.new(0, 270, 0, 300)
ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, 0)
ScrollingFrame.ScrollBarThickness = 5

ToolTemplate.Parent = ScrollingFrame
ToolTemplate.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ToolTemplate.BorderSizePixel = 0
ToolTemplate.Size = UDim2.new(0, 250, 0, 30)
ToolTemplate.Font = Enum.Font.SourceSansBold
ToolTemplate.TextColor3 = Color3.fromRGB(0, 0, 0)
ToolTemplate.TextSize = 18.000
ToolTemplate.Visible = false

SelectedToolFrame.Parent = Frame
SelectedToolFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
SelectedToolFrame.BorderSizePixel = 0
SelectedToolFrame.Position = UDim2.new(0.05, 0, 0.8, 0)
SelectedToolFrame.Size = UDim2.new(0, 270, 0, 50)

SelectedToolLabel.Parent = SelectedToolFrame
SelectedToolLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
SelectedToolLabel.BackgroundTransparency = 1.000
SelectedToolLabel.BorderSizePixel = 0
SelectedToolLabel.Size = UDim2.new(1, 0, 1, 0)
SelectedToolLabel.Font = Enum.Font.SourceSansBold
SelectedToolLabel.Text = "___________"
SelectedToolLabel.TextColor3 = Color3.fromRGB(0, 0, 0)
SelectedToolLabel.TextSize = 20.000

PickButton.Parent = Frame
PickButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
PickButton.BorderSizePixel = 0
PickButton.Position = UDim2.new(0.05, 0, 0.9, 0)
PickButton.Size = UDim2.new(0, 270, 0, 30)
PickButton.Font = Enum.Font.SourceSansBold
PickButton.Text = "Pegar"
PickButton.TextColor3 = Color3.fromRGB(0, 0, 0)
PickButton.TextSize = 20.000

-- Função para listar as Tools
local function listTools()
    for _, tool in pairs(workspace:GetChildren()) do
        if tool:IsA("Tool") then
            local toolButton = ToolTemplate:Clone()
            toolButton.Text = tool.Name
            toolButton.Visible = true
            toolButton.Parent = ScrollingFrame

            toolButton.MouseButton1Click:Connect(function()
                SelectedToolLabel.Text = tool.Name
            end)
        end
    end

    -- Ajusta o tamanho do Canvas para rolar
    ScrollingFrame.CanvasSize = UDim2.new(0, 0, 0, #ScrollingFrame:GetChildren() * 35)
end

-- Função para pegar a Tool selecionada
PickButton.MouseButton1Click:Connect(function()
    local selectedToolName = SelectedToolLabel.Text
    if selectedToolName ~= "___________" then
        local tool = workspace:FindFirstChild(selectedToolName)
        if tool and tool:IsA("Tool") then
            tool:Clone().Parent = game.Players.LocalPlayer.Backpack
        end
    end
end)

-- Inicializa a lista de Tools
listTools()
