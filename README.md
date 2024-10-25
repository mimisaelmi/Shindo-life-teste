-- Script Completo para Mobile no Shindo Life: Auto-Farm e Teleporte com Cooldown

-- Parte 1: Configurações Básicas
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Aumento de Velocidade
humanoid.WalkSpeed = 100 -- Ajuste inicial de velocidade
print("Velocidade definida para " .. humanoid.WalkSpeed)

-- Parte 2: Lista de Bosses e Coordenadas por Aldeia
local bosses = {
    Naruto = Vector3.new(300, 50, 600),
    Sasuke = Vector3.new(320, 50, 640),
    Itachi = Vector3.new(350, 45, 580),
    Gaara = Vector3.new(-250, 20, 900),
    Kankuro = Vector3.new(-200, 25, 950),
    Temari = Vector3.new(-220, 23, 980),
    Zabuza = Vector3.new(100, 20, -400),
    Kisame = Vector3.new(150, 22, -450),
    Raikage = Vector3.new(450, 55, -250),
    Bee = Vector3.new(480, 60, -300),
    Onoki = Vector3.new(-500, 30, 200),
    Deidara = Vector3.new(-530, 28, 250)
}

-- Parte 3: Função de Teleporte para Bosses com Cooldown
local teleportCooldown = false

local function teleportToBoss(bossName)
    if teleportCooldown then
        print("Cooldown ativo! Aguarde para teleportar novamente.")
        return
    end

    if bosses[bossName] then
        character.HumanoidRootPart.CFrame = CFrame.new(bosses[bossName])
        print("Teleportado para o Boss: " .. bossName)

        -- Ativa o cooldown para o teleporte
        teleportCooldown = true
        wait(5) -- Cooldown de 5 segundos
        teleportCooldown = false
    else
        print("Boss '" .. bossName .. "' não encontrado.")
    end
end

-- Parte 4: Auto-Farm com Toggle e Cooldown
local autoFarmEnabled = false
local autoFarmCooldown = false

local function autoFarm()
    while autoFarmEnabled do
        wait(1)
        for _, enemy in pairs(workspace:GetChildren()) do
            if enemy:IsA("Model") and enemy:FindFirstChild("Humanoid") and enemy.Humanoid.Health > 0 then
                character:MoveTo(enemy.HumanoidRootPart.Position)
                wait(0.5)
                humanoid:MoveTo(enemy.HumanoidRootPart.Position)
                humanoid:LoadAnimation(enemy.HumanoidRootPart)
                break
            end
        end
    end
end

-- Toggle para Auto-Farm com cooldown
local function toggleAutoFarm()
    if autoFarmCooldown then
        print("Cooldown ativo! Aguarde para ativar/desativar o Auto-Farm.")
        return
    end
    
    autoFarmEnabled = not autoFarmEnabled
    if autoFarmEnabled then
        print("Auto-Farm ativado!")
        autoFarm()
    else
        print("Auto-Farm desativado!")
    end

    -- Ativa o cooldown para o Auto-Farm
    autoFarmCooldown = true
    wait(5) -- Cooldown de 5 segundos
    autoFarmCooldown = false
end

-- Parte 5: Interface Gráfica (GUI) para Mobile com Cooldown
local ScreenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
local autoFarmButton = Instance.new("TextButton")
autoFarmButton.Size = UDim2.new(0, 150, 0, 50)
autoFarmButton.Position = UDim2.new(0, 10, 0, 10)
autoFarmButton.Text = "Auto-Farm: OFF"
autoFarmButton.Parent = ScreenGui

-- Função para alternar o Auto-Farm
autoFarmButton.MouseButton1Click:Connect(function()
    toggleAutoFarm()
    autoFarmButton.Text = autoFarmEnabled and "Auto-Farm: ON" or "Auto-Farm: OFF"
end)

-- Parte 6: Botões de Teleporte para Bosses com Cooldown
local yOffset = 70
for bossName, _ in pairs(bosses) do
    local bossButton = Instance.new("TextButton")
    bossButton.Size = UDim2.new(0, 150, 0, 50)
    bossButton.Position = UDim2.new(0, 10, 0, yOffset)
    bossButton.Text = "Teleporte: " .. bossName
    bossButton.Parent = ScreenGui
    yOffset = yOffset + 60
    
    bossButton.MouseButton1Click:Connect(function()
        teleportToBoss(bossName)
    end)
end

print("Script carregado! Use os botões na tela para ativar o Auto-Farm e teleportar-se para bosses. Cooldown de 5 segundos ativado.")
