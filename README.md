-- Obter os serviços necessários do Roblox
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Função para criar uma etiqueta de nome para o jogador
local function createESP(player)
    -- Verificar se o personagem do jogador está carregado
    local character = player.Character
    if not character then return end

    -- Espera a parte 'Head' do personagem ser carregada
    local head = character:WaitForChild("Head")

    -- Criar um BillboardGui para exibir o nome acima da cabeça do jogador
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Adornee = head  -- O nome será exibido acima da cabeça
    billboardGui.Size = UDim2.new(0, 200, 0, 50)  -- Define o tamanho do rótulo
    billboardGui.StudsOffset = Vector3.new(0, 3, 0)  -- Posiciona o nome um pouco acima da cabeça

    -- Criar um TextLabel para o nome do jogador
    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = billboardGui
    textLabel.Text = player.Name  -- Define o texto como o nome do jogador
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)  -- Cor do texto (branco)
    textLabel.TextSize = 20  -- Tamanho do texto
    textLabel.BackgroundTransparency = 1  -- Torna o fundo transparente
    textLabel.TextStrokeTransparency = 0.5  -- Borda sutil ao redor do texto para visibilidade

    -- Adiciona o BillboardGui à CoreGui para garantir que ele seja visível
    billboardGui.Parent = game.CoreGui
end

-- Função que roda constantemente para atualizar os jogadores visíveis
RunService.Heartbeat:Connect(function()
    -- Para cada jogador no jogo
    for _, player in ipairs(Players:GetPlayers()) do
        -- Evita que a ESP seja criada para o próprio jogador
        if player ~= Players.LocalPlayer then
            -- Verifica se o ESP já foi criado para esse jogador
            if not player:FindFirstChild("ESP") then
                -- Cria o ESP para o jogador
                createESP(player)
                
                -- Marca o jogador para saber que a ESP já foi criada
                local espTag = Instance.new("BoolValue")
                espTag.Name = "ESP"
                espTag.Parent = player
            end
        end
    end
end)
