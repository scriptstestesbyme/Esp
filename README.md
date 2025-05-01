-- Variables
local teamCheck = true -- Ativar verificação de time
local showESP = true -- Ativar ESP

-- Função para verificar time
function isEnemy(player)
    return player.Team ~= game.Players.LocalPlayer.Team
end

-- Função para desenhar o ESP
function drawESP(player)
    if showESP then
        local character = player.Character
        if character and character:FindFirstChild("HumanoidRootPart") then
            local screenPosition, onScreen = game:GetService("Workspace").CurrentCamera:WorldToScreenPoint(character.HumanoidRootPart.Position)
            if onScreen then
                -- Aqui você pode personalizar o desenho do ESP. Exemplo: um retângulo.
                local size = 20
                local espBox = Instance.new("Frame")
                espBox.Size = UDim2.new(0, size, 0, size)
                espBox.Position = UDim2.new(0, screenPosition.X - size / 2, 0, screenPosition.Y - size / 2)
                espBox.BackgroundColor3 = Color3.fromRGB(255, 0, 0) -- Cor vermelha para inimigos
                espBox.BorderSizePixel = 0
                espBox.Parent = game:GetService("CoreGui")
                -- Remove o ESP após 1 segundo
                game:GetService("Debris"):AddItem(espBox, 1)
            end
        end
    end
end

-- Função para verificar os jogadores e aplicar a lógica
function checkPlayers()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            if teamCheck and isEnemy(player) then
                drawESP(player)
            end
        end
    end
end

-- Atualização da interface
function updateUI()
    local espToggle = Instance.new("TextButton")
    espToggle.Text = "Toggle ESP"
    espToggle.Size = UDim2.new(0, 150, 0, 50)
    espToggle.Position = UDim2.new(0, 50, 0, 50)
    espToggle.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    espToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    espToggle.Parent = game:GetService("CoreGui")

    espToggle.MouseButton1Click:Connect(function()
        showESP = not showESP
    end)

    local teamCheckToggle = Instance.new("TextButton")
    teamCheckToggle.Text = "Toggle Team Check"
    teamCheckToggle.Size = UDim2.new(0, 150, 0, 50)
    teamCheckToggle.Position = UDim2.new(0, 50, 0, 150)
    teamCheckToggle.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
    teamCheckToggle.TextColor3 = Color3.fromRGB(255, 255, 255)
    teamCheckToggle.Parent = game:GetService("CoreGui")

    teamCheckToggle.MouseButton1Click:Connect(function()
        teamCheck = not teamCheck
    end)
end

-- Inicializar a interface
updateUI()

-- Atualizar os jogadores a cada 0.1 segundos
while true do
    wait(0.1)
    checkPlayers()
end
