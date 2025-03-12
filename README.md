-- Configurações SCRIPT

-- Gui to Lua (VIP VERSION)
-- Version: 6.9

-- Instances:

local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Frame_2 = Instance.new("Frame")
local TextLabel = Instance.new("TextLabel")
local ToggleButton = Instance.new("TextButton")

--Properties:

ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.ResetOnSpawn = false

Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(34, 34, 34)
Frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame.BorderSizePixel = 0
Frame.Position = UDim2.new(0.388539821, 0, 0.427821517, 0)
Frame.Size = UDim2.new(0, 250, 0, 100)

Frame_2.Parent = Frame
Frame_2.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
Frame_2.BorderColor3 = Color3.fromRGB(0, 0, 0)
Frame_2.BorderSizePixel = 0
Frame_2.Size = UDim2.new(0, 250, 0, 30)

TextLabel.Parent = Frame_2
TextLabel.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
TextLabel.BackgroundTransparency = 1.000
TextLabel.BorderColor3 = Color3.fromRGB(0, 0, 0)
TextLabel.BorderSizePixel = 0
TextLabel.Position = UDim2.new(0.1, 0, 0, 0)
TextLabel.Size = UDim2.new(0, 200, 0, 30)
TextLabel.Font = Enum.Font.SourceSansBold
TextLabel.Text = "AUTO DETECT"
TextLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
TextLabel.TextSize = 20.000

ToggleButton.Parent = Frame
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.BorderColor3 = Color3.fromRGB(0, 0, 0)
ToggleButton.BorderSizePixel = 0
ToggleButton.Position = UDim2.new(0.1, 0, 0.4, 0)
ToggleButton.Size = UDim2.new(0, 100, 0, 30)
ToggleButton.Font = Enum.Font.SourceSansBold
ToggleButton.Text = "LIGAR"
ToggleButton.TextColor3 = Color3.fromRGB(0, 0, 0)
ToggleButton.TextSize = 20.000

-- Scripts:

local function isBehindWall(target, player)
    local ray = Ray.new(player.Character.HumanoidRootPart.Position, (target.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Unit * 100)
    local hit, position = workspace:FindPartOnRay(ray, player.Character)
    return hit and hit:IsDescendantOf(target.Character)
end

local function autoDetectScript()
    local player = game.Players.LocalPlayer
    local camera = workspace.CurrentCamera
    local systemEnabled = false

    ToggleButton.MouseButton1Click:Connect(function()
        systemEnabled = not systemEnabled
        ToggleButton.Text = systemEnabled and "DESLIGAR" or "LIGAR"
        ToggleButton.TextColor3 = systemEnabled and Color3.fromRGB(255, 0, 0) or Color3.fromRGB(0, 0, 0)
    end)

    while true do
        if systemEnabled then
            local closestPlayer = nil
            local closestDistance = math.huge

            for _, otherPlayer in pairs(game.Players:GetPlayers()) do
                if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("HumanoidRootPart") then
                    local distance = (otherPlayer.Character.HumanoidRootPart.Position - player.Character.HumanoidRootPart.Position).Magnitude
                    if distance < closestDistance and not isBehindWall(otherPlayer, player) then
                        closestDistance = distance
                        closestPlayer = otherPlayer
                    end
                end
            end

            if closestPlayer then
                local targetPosition = closestPlayer.Character.HumanoidRootPart.Position

                -- Entra em primeira pessoa e encara o inimigo
                camera.CameraType = Enum.CameraType.Scriptable
                camera.CFrame = CFrame.new(camera.CFrame.Position, targetPosition)

                -- Atira após 0.5 segundos
                task.wait(0.5)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, true, game, false)
                task.wait(0.1)
                game:GetService("VirtualInputManager"):SendMouseButtonEvent(0, 0, 0, false, game, false)

                -- Volta ao normal
                camera.CameraType = Enum.CameraType.Custom
            end
        end
        task.wait()
    end
end

coroutine.wrap(autoDetectScript)()
