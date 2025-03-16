-- Obtém o jogador e o personagem
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Obtém as partes do corpo
local torso = character:WaitForChild("UpperTorso")
local leftArm = character:WaitForChild("LeftUpperArm")
local rightArm = character:WaitForChild("RightUpperArm")

-- Função para animação idle
local function playIdleAnimation()
    while true do
        -- Movimento do torso para cima e para baixo (simulando respiração)
        for i = 0, 10, 1 do
            torso.CFrame = torso.CFrame * CFrame.new(0, 0.05, 0) -- Move o torso para cima
            leftArm.CFrame = leftArm.CFrame * CFrame.Angles(0, 0, math.rad(1)) -- Rotaciona braço esquerdo
            rightArm.CFrame = rightArm.CFrame * CFrame.Angles(0, 0, math.rad(-1)) -- Rotaciona braço direito
            wait(0.05)
        end
        for i = 0, 10, 1 do
            torso.CFrame = torso.CFrame * CFrame.new(0, -0.05, 0) -- Move o torso para baixo
            leftArm.CFrame = leftArm.CFrame * CFrame.Angles(0, 0, math.rad(-1)) -- Rotaciona braço esquerdo
            rightArm.CFrame = rightArm.CFrame * CFrame.Angles(0, 0, math.rad(1)) -- Rotaciona braço direito
            wait(0.05)
        end
    end
end

-- Inicia a animação idle
playIdleAnimation()
