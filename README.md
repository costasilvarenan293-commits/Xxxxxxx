-- Rennxz Hub ⚡ - FPS Shooting Script
-- KRNL MOBILE COMPATIBLE - SIMPLIFICADO

wait(1)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()

-- Aguardar Character
local Character = Player.Character or Player.CharacterAdded:Wait()
wait(0.5)

local Camera = workspace.CurrentCamera

-- Variáveis
local menuAberto = true
local abaAtual = "Aimbot"
local aimbotAtivo = false
local espAtivo = false
local lineAtivo = false
local jogadorSelecionado = nil
local killAllDesbloqueado = false
local senhaCorreta = "rennxz021"
local tempoExpiracao = 13 * 60 * 60 -- 13 horas em segundos
local tempoDesbloqueio = 0

-- Criar ScreenGui
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RennxzHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = game.CoreGui

-- Frame Principal do Menu (MAIOR)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 350, 0, 400)
MainFrame.Position = UDim2.new(1, -360, 0.5, -200)
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.BorderSizePixel = 0
MainFrame.Active = true
MainFrame.Draggable = false
MainFrame.Parent = ScreenGui

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(255, 0, 0)
MainStroke.Thickness = 3
MainStroke.Parent = MainFrame

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = MainFrame

-- Barra Minimizada
local MinBar = Instance.new("Frame")
MinBar.Name = "MinBar"
MinBar.Size = UDim2.new(0, 200, 0, 45)
MinBar.Position = UDim2.new(0.5, -100, 0, 10)
MinBar.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MinBar.BorderSizePixel = 0
MinBar.Visible = false
MinBar.Active = true
MinBar.Parent = ScreenGui

local MinStroke = Instance.new("UIStroke")
MinStroke.Color = Color3.fromRGB(255, 0, 0)
MinStroke.Thickness = 3
MinStroke.Parent = MinBar

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 10)
MinCorner.Parent = MinBar

local MinIcon = Instance.new("TextLabel")
MinIcon.Size = UDim2.new(1, -40, 1, 0)
MinIcon.Position = UDim2.new(0, 5, 0, 0)
MinIcon.BackgroundTransparency = 1
MinIcon.Text = "Rennxz Hub ⚡"
MinIcon.TextColor3 = Color3.fromRGB(255, 255, 255)
MinIcon.TextSize = 18
MinIcon.Font = Enum.Font.SourceSansBold
MinIcon.TextXAlignment = Enum.TextXAlignment.Left
MinIcon.Parent = MinBar

local OpenButton = Instance.new("TextButton")
OpenButton.Size = UDim2.new(0, 35, 0, 35)
OpenButton.Position = UDim2.new(1, -40, 0.5, -17.5)
OpenButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
OpenButton.BorderSizePixel = 0
OpenButton.Text = "+"
OpenButton.TextColor3 = Color3.fromRGB(255, 0, 0)
OpenButton.TextSize = 22
OpenButton.Font = Enum.Font.SourceSansBold
OpenButton.Parent = MinBar

local OpenStroke = Instance.new("UIStroke")
OpenStroke.Color = Color3.fromRGB(255, 0, 0)
OpenStroke.Thickness = 2
OpenStroke.Parent = OpenButton

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0, 8)
OpenCorner.Parent = OpenButton

-- Título com Efeito RGB
local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.BackgroundTransparency = 1
Title.Text = "rennxz hub ⚡"
Title.TextSize = 22
Title.Font = Enum.Font.SourceSansBold
Title.TextColor3 = Color3.fromRGB(255, 0, 0)
Title.Parent = MainFrame

-- Botão Fechar
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
CloseButton.BorderSizePixel = 0
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 0, 0)
CloseButton.TextSize = 18
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.Parent = MainFrame

local CloseStroke = Instance.new("UIStroke")
CloseStroke.Color = Color3.fromRGB(255, 0, 0)
CloseStroke.Thickness = 2
CloseStroke.Parent = CloseButton

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 6)
CloseCorner.Parent = CloseButton

-- Container de Abas
local TabContainer = Instance.new("Frame")
TabContainer.Size = UDim2.new(1, -20, 0, 35)
TabContainer.Position = UDim2.new(0, 10, 0, 45)
TabContainer.BackgroundTransparency = 1
TabContainer.Parent = MainFrame

-- Container de Conteúdo
local ContentContainer = Instance.new("Frame")
ContentContainer.Size = UDim2.new(1, -20, 1, -95)
ContentContainer.Position = UDim2.new(0, 10, 0, 85)
ContentContainer.BackgroundTransparency = 1
ContentContainer.Parent = MainFrame

-- Função para criar abas
local function CriarAba(nome, posicao)
    local Aba = Instance.new("TextButton")
    Aba.Name = nome
    Aba.Size = UDim2.new(0.25, -5, 1, 0)
    Aba.Position = UDim2.new(0.25 * posicao, 0, 0, 0)
    Aba.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Aba.BorderSizePixel = 0
    Aba.Text = nome
    Aba.TextColor3 = Color3.fromRGB(255, 255, 255)
    Aba.TextSize = 14
    Aba.Font = Enum.Font.SourceSansBold
    Aba.Parent = TabContainer
    
    local AbaStroke = Instance.new("UIStroke")
    AbaStroke.Color = Color3.fromRGB(255, 0, 0)
    AbaStroke.Thickness = 2
    AbaStroke.Parent = Aba
    
    local AbaCorner = Instance.new("UICorner")
    AbaCorner.CornerRadius = UDim.new(0, 6)
    AbaCorner.Parent = Aba
    
    return Aba
end

-- Criar Abas
local AbaAimbot = CriarAba("Aimbot", 0)
local AbaVisual = CriarAba("Visual", 1)
local AbaTP = CriarAba("TP", 2)
local AbaKillAll = CriarAba("Kill All", 3)

-- Função para criar botão
local function CriarBotao(nome, posicao, parent, funcao)
    local Botao = Instance.new("TextButton")
    Botao.Name = nome
    Botao.Size = UDim2.new(1, 0, 0, 35)
    Botao.Position = UDim2.new(0, 0, 0, posicao)
    Botao.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    Botao.BorderSizePixel = 0
    Botao.Text = nome
    Botao.TextColor3 = Color3.fromRGB(255, 255, 255)
    Botao.TextSize = 14
    Botao.Font = Enum.Font.SourceSansBold
    Botao.Parent = parent
    
    local BotaoStroke = Instance.new("UIStroke")
    BotaoStroke.Color = Color3.fromRGB(255, 0, 0)
    BotaoStroke.Thickness = 2
    BotaoStroke.Parent = Botao
    
    local BotaoCorner = Instance.new("UICorner")
    BotaoCorner.CornerRadius = UDim.new(0, 8)
    BotaoCorner.Parent = Botao
    
    if funcao then
        Botao.MouseButton1Click:Connect(funcao)
    end
    
    return Botao
end

-- Função para criar slider
local function CriarSlider(nome, posicao, parent, minVal, maxVal, valorInicial, callback)
    local Container = Instance.new("Frame")
    Container.Size = UDim2.new(1, 0, 0, 50)
    Container.Position = UDim2.new(0, 0, 0, posicao)
    Container.BackgroundTransparency = 1
    Container.Parent = parent
    
    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 0, 20)
    Label.BackgroundTransparency = 1
    Label.Text = nome .. ": " .. valorInicial
    Label.TextColor3 = Color3.fromRGB(255, 255, 255)
    Label.TextSize = 14
    Label.Font = Enum.Font.SourceSansBold
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Container
    
    local SliderFrame = Instance.new("Frame")
    SliderFrame.Size = UDim2.new(1, 0, 0, 20)
    SliderFrame.Position = UDim2.new(0, 0, 0, 25)
    SliderFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    SliderFrame.Parent = Container
    
    local SliderCorner = Instance.new("UICorner")
    SliderCorner.CornerRadius = UDim.new(0, 5)
    SliderCorner.Parent = SliderFrame
    
    local SliderFill = Instance.new("Frame")
    SliderFill.Size = UDim2.new((valorInicial - minVal) / (maxVal - minVal), 0, 1, 0)
    SliderFill.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    SliderFill.Parent = SliderFrame
    
    local FillCorner = Instance.new("UICorner")
    FillCorner.CornerRadius = UDim.new(0, 5)
    FillCorner.Parent = SliderFill
    
    local dragging = false
    
    SliderFrame.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
        end
    end)
    
    SliderFrame.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            local mousePos = UserInputService:GetMouseLocation().X
            local framePos = SliderFrame.AbsolutePosition.X
            local frameSize = SliderFrame.AbsoluteSize.X
            
            local percentage = math.clamp((mousePos - framePos) / frameSize, 0, 1)
            local value = math.floor(minVal + (percentage * (maxVal - minVal)))
            
            SliderFill.Size = UDim2.new(percentage, 0, 1, 0)
            Label.Text = nome .. ": " .. value
            
            if callback then
                callback(value)
            end
        end
    end)
    
    return Container
end

-- Conteúdo das Abas
local ConteudoAimbot = Instance.new("ScrollingFrame")
ConteudoAimbot.Size = UDim2.new(1, 0, 1, 0)
ConteudoAimbot.BackgroundTransparency = 1
ConteudoAimbot.ScrollBarThickness = 6
ConteudoAimbot.Visible = true
ConteudoAimbot.Parent = ContentContainer

local ConteudoVisual = Instance.new("ScrollingFrame")
ConteudoVisual.Size = UDim2.new(1, 0, 1, 0)
ConteudoVisual.BackgroundTransparency = 1
ConteudoVisual.ScrollBarThickness = 6
ConteudoVisual.Visible = false
ConteudoVisual.Parent = ContentContainer

local ConteudoTP = Instance.new("ScrollingFrame")
ConteudoTP.Size = UDim2.new(1, 0, 1, 0)
ConteudoTP.BackgroundTransparency = 1
ConteudoTP.ScrollBarThickness = 6
ConteudoTP.Visible = false
ConteudoTP.Parent = ContentContainer

local ConteudoKillAll = Instance.new("ScrollingFrame")
ConteudoKillAll.Size = UDim2.new(1, 0, 1, 0)
ConteudoKillAll.BackgroundTransparency = 1
ConteudoKillAll.ScrollBarThickness = 6
ConteudoKillAll.Visible = false
ConteudoKillAll.Parent = ContentContainer

-- Sistema de Abas
local function MostrarAba(aba)
    ConteudoAimbot.Visible = false
    ConteudoVisual.Visible = false
    ConteudoTP.Visible = false
    ConteudoKillAll.Visible = false
    
    -- Resetar cores das abas
    AbaAimbot.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    AbaVisual.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    AbaTP.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    AbaKillAll.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    
    if aba == "Aimbot" then
        ConteudoAimbot.Visible = true
        AbaAimbot.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    elseif aba == "Visual" then
        ConteudoVisual.Visible = true
        AbaVisual.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    elseif aba == "TP" then
        ConteudoTP.Visible = true
        AbaTP.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    elseif aba == "Kill All" then
        ConteudoKillAll.Visible = true
        AbaKillAll.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    end
end

AbaAimbot.MouseButton1Click:Connect(function() MostrarAba("Aimbot") end)
AbaVisual.MouseButton1Click:Connect(function() MostrarAba("Visual") end)
AbaTP.MouseButton1Click:Connect(function() MostrarAba("TP") end)
AbaKillAll.MouseButton1Click:Connect(function() MostrarAba("Kill All") end)

-- Inicializar primeira aba
MostrarAba("Aimbot")

-- ABA AIMBOT
local BotaoAimbot = CriarBotao("Aimbot", 0, ConteudoAimbot, function()
    aimbotAtivo = not aimbotAtivo
    
    if aimbotAtivo then
        BotaoAimbot.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        local stroke = BotaoAimbot:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(0, 255, 0)
        end
    else
        BotaoAimbot.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        local stroke = BotaoAimbot:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(255, 0, 0)
        end
    end
end)

CriarSlider("FOV Camera", 45, ConteudoAimbot, 70, 120, 70, function(value)
    fovAtual = value
    Camera.FieldOfView = value
end)

-- ABA VISUAL
local BotaoESP = CriarBotao("ESP", 0, ConteudoVisual, function()
    espAtivo = not espAtivo
    
    if espAtivo then
        BotaoESP.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        local stroke = BotaoESP:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(0, 255, 0)
        end
    else
        BotaoESP.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        local stroke = BotaoESP:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(255, 0, 0)
        end
        
        -- Limpar ESP existente
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player and player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BoxHandleAdornment") or part:IsA("BillboardGui") then
                        part:Destroy()
                    end
                end
            end
        end
    end
end)

local BotaoLine = CriarBotao("Tracer Line", 40, ConteudoVisual, function()
    lineAtivo = not lineAtivo
    
    if lineAtivo then
        BotaoLine.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
        local stroke = BotaoLine:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(0, 255, 0)
        end
    else
        BotaoLine.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
        local stroke = BotaoLine:FindFirstChildOfClass("UIStroke")
        if stroke then
            stroke.Color = Color3.fromRGB(255, 0, 0)
        end
    end
end)

-- ABA TP
local ListaJogadores = Instance.new("ScrollingFrame")
ListaJogadores.Size = UDim2.new(1, 0, 0, 200)
ListaJogadores.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ListaJogadores.BorderSizePixel = 0
ListaJogadores.ScrollBarThickness = 6
ListaJogadores.Parent = ConteudoTP

local ListaCorner = Instance.new("UICorner")
ListaCorner.CornerRadius = UDim.new(0, 8)
ListaCorner.Parent = ListaJogadores

local BotaoTPJogador = CriarBotao("TP para Jogador Selecionado", 210, ConteudoTP, function()
    if jogadorSelecionado and Character and Character:FindFirstChild("HumanoidRootPart") then
        local targetPlayer = Players:FindFirstChild(jogadorSelecionado)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
        end
    end
end)

-- Atualizar lista de jogadores
spawn(function()
    while wait(2) do
        for _, child in pairs(ListaJogadores:GetChildren()) do
            if child:IsA("TextButton") then
                child:Destroy()
            end
        end
        
        local yPos = 0
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player then
                local BotaoJogador = Instance.new("TextButton")
                BotaoJogador.Size = UDim2.new(1, -10, 0, 30)
                BotaoJogador.Position = UDim2.new(0, 5, 0, yPos)
                BotaoJogador.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                BotaoJogador.Text = player.Name
                BotaoJogador.TextColor3 = Color3.fromRGB(255, 255, 255)
                BotaoJogador.TextSize = 14
                BotaoJogador.Font = Enum.Font.SourceSansBold
                BotaoJogador.Parent = ListaJogadores
                
                local BotaoCorner = Instance.new("UICorner")
                BotaoCorner.CornerRadius = UDim.new(0, 6)
                BotaoCorner.Parent = BotaoJogador
                
                BotaoJogador.MouseButton1Click:Connect(function()
                    jogadorSelecionado = player.Name
                    
                    for _, btn in pairs(ListaJogadores:GetChildren()) do
                        if btn:IsA("TextButton") then
                            btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
                        end
                    end
                    
                    BotaoJogador.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
                end)
                
                yPos = yPos + 35
            end
        end
        
        ListaJogadores.CanvasSize = UDim2.new(0, 0, 0, yPos)
    end
end)

-- ABA KILL ALL - COM SISTEMA DE KEY E TIMER
-- Verificar se o key expirou
if killAllDesbloqueado and (tick() - tempoDesbloqueio) > tempoExpiracao then
    killAllDesbloqueado = false
end

if not killAllDesbloqueado then
    -- Criar tela de senha
    local SenhaFrame = Instance.new("Frame")
    SenhaFrame.Size = UDim2.new(1, 0, 1, 0)
    SenhaFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    SenhaFrame.BorderSizePixel = 0
    SenhaFrame.Parent = ConteudoKillAll
    
    local SenhaCorner = Instance.new("UICorner")
    SenhaCorner.CornerRadius = UDim.new(0, 10)
    SenhaCorner.Parent = SenhaFrame
    
    local IconeLock = Instance.new("TextLabel")
    IconeLock.Size = UDim2.new(1, 0, 0, 60)
    IconeLock.Position = UDim2.new(0, 0, 0, 10)
    IconeLock.BackgroundTransparency = 1
    IconeLock.Text = "🔒"
    IconeLock.TextSize = 50
    IconeLock.Font = Enum.Font.SourceSansBold
    IconeLock.Parent = SenhaFrame
    
    local TituloSenha = Instance.new("TextLabel")
    TituloSenha.Size = UDim2.new(1, -40, 0, 30)
    TituloSenha.Position = UDim2.new(0, 20, 0, 75)
    TituloSenha.BackgroundTransparency = 1
    TituloSenha.Text = "Kill All Bloqueado"
    TituloSenha.TextColor3 = Color3.fromRGB(255, 255, 255)
    TituloSenha.TextSize = 18
    TituloSenha.Font = Enum.Font.SourceSansBold
    TituloSenha.Parent = SenhaFrame
    
    local TextoInfo = Instance.new("TextLabel")
    TextoInfo.Size = UDim2.new(1, -40, 0, 50)
    TextoInfo.Position = UDim2.new(0, 20, 0, 110)
    TextoInfo.BackgroundTransparency = 1
    TextoInfo.Text = "Digite a senha para desbloquear\n(Válido por 13 horas):"
    TextoInfo.TextColor3 = Color3.fromRGB(200, 200, 200)
    TextoInfo.TextSize = 14
    TextoInfo.Font = Enum.Font.SourceSans
    TextoInfo.TextWrapped = true
    TextoInfo.Parent = SenhaFrame
    
    local InputSenha = Instance.new("TextBox")
    InputSenha.Size = UDim2.new(1, -60, 0, 40)
    InputSenha.Position = UDim2.new(0, 30, 0, 170)
    InputSenha.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    InputSenha.BorderSizePixel = 0
    InputSenha.Text = ""
    InputSenha.PlaceholderText = "Digite a senha..."
    InputSenha.TextColor3 = Color3.fromRGB(255, 255, 255)
    InputSenha.PlaceholderColor3 = Color3.fromRGB(150, 150, 150)
    InputSenha.TextSize = 16
    InputSenha.Font = Enum.Font.SourceSans
    InputSenha.ClearTextOnFocus = false
    InputSenha.Parent = SenhaFrame
    
    local InputCorner = Instance.new("UICorner")
    InputCorner.CornerRadius = UDim.new(0, 8)
    InputCorner.Parent = InputSenha
    
    local BotaoVerificar = Instance.new("TextButton")
    BotaoVerificar.Size = UDim2.new(1, -60, 0, 40)
    BotaoVerificar.Position = UDim2.new(0, 30, 0, 220)
    BotaoVerificar.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
    BotaoVerificar.BorderSizePixel = 0
    BotaoVerificar.Text = "Desbloquear"
    BotaoVerificar.TextColor3 = Color3.fromRGB(255, 255, 255)
    BotaoVerificar.TextSize = 16
    BotaoVerificar.Font = Enum.Font.SourceSansBold
    BotaoVerificar.Parent = SenhaFrame
    
    local BotaoStroke = Instance.new("UIStroke")
    BotaoStroke.Color = Color3.fromRGB(255, 0, 0)
    BotaoStroke.Thickness = 2
    BotaoStroke.Parent = BotaoVerificar
    
    local BotaoCorner = Instance.new("UICorner")
    BotaoCorner.Co
