-- RENNXZ HUB - STEAL
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

local Window = Rayfield:CreateWindow({
    Name = "RENNXZ HUB - STEAL",
    LoadingTitle = "RENNXZ HUB",
    LoadingSubtitle = "por RENNXZ",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "RENNXZHub",
        FileName = "StealConfig"
    },
})

-- Variáveis globais
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
local savedPosition = nil
local noclipEnabled = false
local stealFloorEnabled = false
local espEnabled = false
local stealFloorPart = nil
local espFolder = Instance.new("Folder", game.CoreGui)
espFolder.Name = "ESPFolder"
local originalTransparency = {} -- Guardar transparência original das partes

-- Atualizar character quando respawnar
player.CharacterAdded:Connect(function(char)
    character = char
    humanoidRootPart = char:WaitForChild("HumanoidRootPart")
end)

-- Tab Principal
local MainTab = Window:CreateTab("Principal", 4483362458)

-- Seção de Teleporte
local TeleportSection = MainTab:CreateSection("Teleporte")

MainTab:CreateButton({
    Name = "TP para Base",
    Callback = function()
        if savedPosition then
            -- TP rápido mas não instantâneo
            local targetPos = savedPosition
            local currentPos = humanoidRootPart.CFrame
            local distance = (targetPos.Position - currentPos.Position).Magnitude
            local duration = math.clamp(distance / 100, 0.3, 2) -- Velocidade ajustável
            
            local startTime = tick()
            local connection
            connection = game:GetService("RunService").Heartbeat:Connect(function()
                local elapsed = tick() - startTime
                local alpha = math.min(elapsed / duration, 1)
                
                if character and humanoidRootPart then
                    humanoidRootPart.CFrame = currentPos:Lerp(targetPos, alpha)
                end
                
                if alpha >= 1 then
                    connection:Disconnect()
                    Rayfield:Notify({
                        Title = "Teleportado!",
                        Content = "Você chegou na base",
                        Duration = 2,
                        Image = 4483362458,
                    })
                end
            end)
        else
            Rayfield:Notify({
                Title = "Erro!",
                Content = "Nenhuma base foi salva ainda",
                Duration = 3,
                Image = 4483362458,
            })
        end
    end,
})

MainTab:CreateButton({
    Name = "Salvar Base",
    Callback = function()
        savedPosition = humanoidRootPart.CFrame
        Rayfield:Notify({
            Title = "Base Salva!",
            Content = "Posição atual salva com sucesso",
            Duration = 3,
            Image = 4483362458,
        })
    end,
})

-- Seção de Movimento
local MovementSection = MainTab:CreateSection("Movimento")

MainTab:CreateButton({
    Name = "Noclip (TP Frontal)",
    Callback = function()
        if character and humanoidRootPart then
            local lookDirection = humanoidRootPart.CFrame.LookVector
            local newPosition = humanoidRootPart.CFrame + (lookDirection * 5) -- 5 studs para frente
            humanoidRootPart.CFrame = newPosition
            
            Rayfield:Notify({
                Title = "Noclip!",
                Content = "Teleportado para frente",
                Duration = 1,
                Image = 4483362458,
            })
        end
    end,
})

MainTab:CreateToggle({
    Name = "Steal Floor (Plataforma + Ver através das paredes)",
    CurrentValue = false,
    Flag = "StealFloorToggle",
    Callback = function(value)
        stealFloorEnabled = value
        
        if value then
            -- Criar plataforma
            stealFloorPart = Instance.new("Part")
            stealFloorPart.Name = "StealFloor"
            stealFloorPart.Size = Vector3.new(8, 1, 8)
            stealFloorPart.Anchored = true
            stealFloorPart.CanCollide = true
            stealFloorPart.Transparency = 0.5
            stealFloorPart.BrickColor = BrickColor.new("Bright blue")
            stealFloorPart.Material = Enum.Material.Neon
            stealFloorPart.Parent = workspace
            
            -- Tornar paredes transparentes (exceto chão)
            for _, obj in pairs(workspace:GetDescendants()) do
                if obj:IsA("BasePart") and obj ~= stealFloorPart then
                    -- Verificar se não é chão (partes horizontais embaixo do jogador)
                    local isFloor = false
                    if obj.Size.Y < obj.Size.X or obj.Size.Y < obj.Size.Z then
                        local playerY = humanoidRootPart.Position.Y
                        local partY = obj.Position.Y
                        if partY < playerY - 3 then
                            isFloor = true
                        end
                    end
                    
                    -- Se não for chão, tornar transparente
                    if not isFloor then
                        -- Salvar transparência original
                        originalTransparency[obj] = obj.Transparency
                        obj.Transparency = 0.8
                    end
                end
            end
            
            -- Loop para mover a plataforma
            spawn(function()
                while stealFloorEnabled and character and humanoidRootPart do
                    wait(0.03)
                    if stealFloorPart then
                        local newPos = humanoidRootPart.Position - Vector3.new(0, 3, 0)
                        stealFloorPart.Position = newPos
                        
                        -- Mover jogador para cima
                        humanoidRootPart.Velocity = Vector3.new(0, 50, 0)
                    end
                end
            end)
            
            Rayfield:Notify({
                Title = "Steal Floor Ativado!",
                Content = "Plataforma criada e paredes transparentes",
                Duration = 3,
                Image = 4483362458,
            })
        else
            -- Remover plataforma
            if stealFloorPart then
                stealFloorPart:Destroy()
                stealFloorPart = nil
            end
            
            -- Restaurar transparência original das paredes
            for obj, originalValue in pairs(originalTransparency) do
                if obj and obj.Parent then
                    obj.Transparency = originalValue
                end
            end
            originalTransparency = {} -- Limpar tabela
            
            Rayfield:Notify({
                Title = "Steal Floor Desativado!",
                Content = "Plataforma removida e paredes restauradas",
                Duration = 2,
                Image = 4483362458,
            })
        end
    end,
})

-- Seção de ESP
local ESPSection = MainTab:CreateSection("ESP")

local function createESP(targetPlayer)
    if targetPlayer == player then return end
    
    local char = targetPlayer.Character
    if not char then return end
    
    local highlight = Instance.new("Highlight")
    highlight.Name = "ESP_" .. targetPlayer.Name
    highlight.Adornee = char
    highlight.FillColor = Color3.fromRGB(255, 0, 0)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    highlight.Parent = espFolder
    
    -- BillboardGui com nome
    local billboard = Instance.new("BillboardGui")
    billboard.Name = "ESP_Billboard"
    billboard.Adornee = char:FindFirstChild("Head")
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    billboard.Parent = espFolder
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.Text = targetPlayer.Name
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextStrokeTransparency = 0.5
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextSize = 14
    textLabel.Parent = billboard
end

local function removeESP(targetPlayer)
    for _, obj in pairs(espFolder:GetChildren()) do
        if obj.Name:find(targetPlayer.Name) then
            obj:Destroy()
        end
    end
end

MainTab:CreateToggle({
    Name = "ESP (Ver Jogadores)",
    CurrentValue = false,
    Flag = "ESPToggle",
    Callback = function(value)
        espEnabled = value
        
        if value then
            -- Criar ESP para todos os jogadores
            for _, targetPlayer in pairs(game.Players:GetPlayers()) do
                if targetPlayer.Character then
                    createESP(targetPlayer)
                end
            end
            
            -- ESP para novos jogadores
            game.Players.PlayerAdded:Connect(function(newPlayer)
                if espEnabled then
                    newPlayer.CharacterAdded:Connect(function()
                        wait(1)
                        if espEnabled then
                            createESP(newPlayer)
                        end
                    end)
                end
            end)
            
            Rayfield:Notify({
                Title = "ESP Ativado!",
                Content = "Você pode ver todos os jogadores",
                Duration = 3,
                Image = 4483362458,
            })
        else
            -- Remover todos os ESPs
            espFolder:ClearAllChildren()
            
            Rayfield:Notify({
                Title = "ESP Desativado!",
                Content = "ESP removido",
                Duration = 2,
                Image = 4483362458,
            })
        end
    end,
})

Rayfield:LoadConfiguration()
