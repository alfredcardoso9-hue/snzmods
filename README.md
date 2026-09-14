# snzmods
script snzmods 
--[[
    ═══════════════════════════════════════════
    SNZMODS - Painel Universal (Delta Executor)
    Compatível com Mobile e Desktop
    ═══════════════════════════════════════════
--]]

-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera

-- ═══════════════════════════════════════════
-- PROTEÇÃO CONTRA DUPLICATAS
-- ═══════════════════════════════════════════
if CoreGui:FindFirstChild("snzmods") then
    CoreGui.snzmods:Destroy()
end
if gethui then
    local hui = gethui()
    if hui:FindFirstChild("snzmods") then
        hui.snzmods:Destroy()
    end
end

-- ═══════════════════════════════════════════
-- CONFIGURAÇÕES GERAIS
-- ═══════════════════════════════════════════
local Config = {
    ESP = {
        Enabled = false,
        Boxes = true,
        Names = true,
        Tracers = false,
        Health = true,
        Distance = true,
        MaxDistance = 1000,
        TeamCheck = false,
        Color = Color3.fromRGB(0, 170, 255)
    },
    Aimbot = {
        Enabled = false,
        TeamCheck = true,
        FOV = 150,
        Smoothness = 0.15,
        VisibleCheck = false,
        Key = Enum.UserInputType.MouseButton2
    }
}

-- ═══════════════════════════════════════════
-- FUNÇÃO AUXILIAR: obter GUI parent
-- ═══════════════════════════════════════════
local function getGuiParent()
    if gethui then
        return gethui()
    elseif RunService:IsStudio() then
        return LocalPlayer:WaitForChild("PlayerGui")
    else
        return CoreGui
    end
end

-- ═══════════════════════════════════════════
-- CRIAÇÃO DA INTERFACE
-- ═══════════════════════════════════════════
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "snzmods"
ScreenGui.ResetOnSpawn = false
ScreenGui.IgnoreGuiInset = true
ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
ScreenGui.Parent = getGuiParent()

-- ─── Container de ESP (fora do painel) ───
local ESPFolder = Instance.new("Folder")
ESPFolder.Name = "ESP"
ESPFolder.Parent = ScreenGui

-- ─── Painel Principal ───
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.new(0, 320, 0, 420)
Main.Position = UDim2.new(0.5, -160, 0.5, -210)
Main.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
Main.BorderSizePixel = 0
Main.Active = true
Main.Draggable = true
Main.Parent = ScreenGui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 14)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = Color3.fromRGB(45, 45, 60)
MainStroke.Thickness = 1.5
MainStroke.Parent = Main

-- ─── Header ───
local Header = Instance.new("Frame")
Header.Name = "Header"
Header.Size = UDim2.new(1, 0, 0, 45)
Header.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
Header.BorderSizePixel = 0
Header.Parent = Main

local HeaderCorner = Instance.new("UICorner")
HeaderCorner.CornerRadius = UDim.new(0, 14)
HeaderCorner.Parent = Header

local HeaderFix = Instance.new("Frame")
HeaderFix.Size = UDim2.new(1, 0, 0, 14)
HeaderFix.Position = UDim2.new(0, 0, 1, -14)
HeaderFix.BackgroundColor3 = Color3.fromRGB(24, 24, 34)
HeaderFix.BorderSizePixel = 0
HeaderFix.Parent = Header

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Size = UDim2.new(1, -80, 1, 0)
Title.Position = UDim2.new(0, 15, 0, 0)
Title.BackgroundTransparency = 1
Title.Text = "snzmods"
Title.TextColor3 = Color3.fromRGB(0, 200, 255)
Title.TextSize = 22
Title.Font = Enum.Font.GothamBold
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Header

-- Botão Minimizar
local MinBtn = Instance.new("TextButton")
MinBtn.Name = "MinBtn"
MinBtn.Size = UDim2.new(0, 30, 0, 30)
MinBtn.Position = UDim2.new(1, -70, 0.5, -15)
MinBtn.BackgroundColor3 = Color3.fromRGB(35, 35, 45)
MinBtn.Text = "—"
MinBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
MinBtn.TextSize = 18
MinBtn.Font = Enum.Font.GothamBold
MinBtn.AutoButtonColor = true
MinBtn.Parent = Header

local MinCorner = Instance.new("UICorner")
MinCorner.CornerRadius = UDim.new(0, 8)
MinCorner.Parent = MinBtn

-- Botão Fechar
local CloseBtn = Instance.new("TextButton")
CloseBtn.Name = "CloseBtn"
CloseBtn.Size = UDim2.new(0, 30, 0, 30)
CloseBtn.Position = UDim2.new(1, -35, 0.5, -15)
CloseBtn.BackgroundColor3 = Color3.fromRGB(60, 25, 25)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255, 120, 120)
CloseBtn.TextSize = 16
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.Parent = Header

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 8)
CloseCorner.Parent = CloseBtn

-- ─── Container de Páginas ───
local PageContainer = Instance.new("Frame")
PageContainer.Name = "PageContainer"
PageContainer.Size = UDim2.new(1, -20, 1, -100)
PageContainer.Position = UDim2.new(0, 10, 0, 55)
PageContainer.BackgroundTransparency = 1
PageContainer.Parent = Main

-- ─── Navegação (Tabs) ───
local NavBar = Instance.new("Frame")
NavBar.Name = "NavBar"
NavBar.Size = UDim2.new(1, -20, 0, 35)
NavBar.Position = UDim2.new(0, 10, 1, -45)
NavBar.BackgroundTransparency = 1
NavBar.Parent = Main

local NavLayout = Instance.new("UIListLayout")
NavLayout.FillDirection = Enum.FillDirection.Horizontal
NavLayout.Padding = UDim.new(0, 6)
NavLayout.SortOrder = Enum.SortOrder.LayoutOrder
NavLayout.Parent = NavBar

-- ═══════════════════════════════════════════
-- CRIAÇÃO DE PÁGINAS
-- ═══════════════════════════════════════════
local Pages = {}

local function CreatePage(name)
    local Page = Instance.new("ScrollingFrame")
    Page.Name = name .. "Page"
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.BackgroundTransparency = 1
    Page.BorderSizePixel = 0
    Page.ScrollBarThickness = 4
    Page.ScrollBarImageColor3 = Color3.fromRGB(0, 170, 255)
    Page.CanvasSize = UDim2.new(0, 0, 0, 0)
    Page.AutomaticCanvasSize = Enum.AutomaticSize.Y
    Page.Visible = false
    Page.Parent = PageContainer

    local Layout = Instance.new("UIListLayout")
    Layout.Padding = UDim.new(0, 8)
    Layout.SortOrder = Enum.SortOrder.LayoutOrder
    Layout.Parent = Page

    local Pad = Instance.new("UIPadding")
    Pad.PaddingTop = UDim.new(0, 5)
    Pad.PaddingBottom = UDim.new(0, 5)
    Pad.Parent = Page

    Pages[name] = Page
    return Page
end

-- ─── Criação de Botões de Navegação ───
local Tabs = {}
local function CreateTab(name)
    local Btn = Instance.new("TextButton")
    Btn.Name = name .. "Tab"
    Btn.Size = UDim2.new(0.33, -4, 1, 0)
    Btn.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
    Btn.Text = name
    Btn.TextColor3 = Color3.fromRGB(180, 180, 190)
    Btn.TextSize = 13
    Btn.Font = Enum.Font.GothamMedium
    Btn.Parent = NavBar

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 8)
    Corner.Parent = Btn

    Tabs[name] = Btn
    return Btn
end

-- ─── Botão Toggle (estilo Switch) ───
local function CreateToggle(parent, text, default, callback)
    local Holder = Instance.new("Frame")
    Holder.Name = text .. "Toggle"
    Holder.Size = UDim2.new(1, 0, 0, 42)
    Holder.BackgroundColor3 = Color3.fromRGB(26, 26, 34)
    Holder.BorderSizePixel = 0
    Holder.Parent = parent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Holder

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -70, 1, 0)
    Label.Position = UDim2.new(0, 12, 0, 0)
    Label.BackgroundTransparency = 1
    Label.Text = text
    Label.TextColor3 = Color3.fromRGB(220, 220, 230)
    Label.TextSize = 14
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Holder

    local Switch = Instance.new("TextButton")
    Switch.Size = UDim2.new(0, 46, 0, 24)
    Switch.Position = UDim2.new(1, -58, 0.5, -12)
    Switch.BackgroundColor3 = default and Color3.fromRGB(0, 170, 255) or Color3.fromRGB(55, 55, 65)
    Switch.Text = ""
    Switch.AutoButtonColor = false
    Switch.Parent = Holder

    local SwitchCorner = Instance.new("UICorner")
    SwitchCorner.CornerRadius = UDim.new(1, 0)
    SwitchCorner.Parent = Switch

    local Knob = Instance.new("Frame")
    Knob.Size = UDim2.new(0, 18, 0, 18)
    Knob.Position = default and UDim2.new(1, -21, 0.5, -9) or UDim2.new(0, 3, 0.5, -9)
    Knob.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
    Knob.Parent = Switch

    local KnobCorner = Instance.new("UICorner")
    KnobCorner.CornerRadius = UDim.new(1, 0)
    KnobCorner.Parent = Knob

    local state = default

    Switch.MouseButton1Click:Connect(function()
        state = not state
        Switch.BackgroundColor3 = state and Color3.fromRGB(0, 170, 255) or Color3.fromRGB(55, 55, 65)
        Knob:TweenPosition(
            state and UDim2.new(1, -21, 0.5, -9) or UDim2.new(0, 3, 0.5, -9),
            Enum.EasingDirection.Out,
            Enum.EasingStyle.Quad,
            0.15,
            true
        )
        if callback then
            callback(state)
        end
    end)

    return Holder, function() return state end
end

-- ─── Botão Slider ───
local function CreateSlider(parent, text, min, max, default, callback)
    local Holder = Instance.new("Frame")
    Holder.Size = UDim2.new(1, 0, 0, 55)
    Holder.BackgroundColor3 = Color3.fromRGB(26, 26, 34)
    Holder.BorderSizePixel = 0
    Holder.Parent = parent

    local Corner = Instance.new("UICorner")
    Corner.CornerRadius = UDim.new(0, 10)
    Corner.Parent = Holder

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, -20, 0, 20)
    Label.Position = UDim2.new(0, 12, 0, 5)
    Label.BackgroundTransparency = 1
    Label.Text = text .. ": " .. default
    Label.TextColor3 = Color3.fromRGB(220, 220, 230)
    Label.TextSize = 13
    Label.Font = Enum.Font.GothamMedium
    Label.TextXAlignment = Enum.TextXAlignment.Left
    Label.Parent = Holder

    local Bar = Instance.new("Frame")
    Bar.Size = UDim2.new(1, -24, 0, 6)
    Bar.Position = UDim2.new(0, 12, 1, -18)
    Bar.BackgroundColor3 = Color3.fromRGB(45, 45, 55)
    Bar.Parent = Holder

    local BarCorner = Instance.new("UICorner")
    BarCorner.CornerRadius = UDim.new(1, 0)
    BarCorner.Parent = Bar

    local Fill = Instance.new("Frame")
    Fill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
    Fill.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
    Fill.Parent = Bar

    local FillCorner = Instance.new("UICorner")
    FillCorner.CornerRadius = UDim.new(1, 0)
    FillCorner.Parent = Fill

    local value = default
    local dragging = false

    local function update(input)
        local pos = math.clamp((input.Position.X - Bar.AbsolutePosition.X) / Bar.AbsoluteSize.X, 0, 1)
        value = math.floor(min + (max - min) * pos + 0.5)
        Fill.Size = UDim2.new(pos, 0, 1, 0)
        Label.Text = text .. ": " .. value
        if callback then callback(value) end
    end

    Bar.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            update(input)
        end
    end)

    Bar.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = false
        end
    end)

    UserInputService.InputChanged:Connect(function(input)
        if dragging and (input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch) then
            update(input)
        end
    end)

    return Holder, function() return value end
end

-- ═══════════════════════════════════════════
-- CONTEÚDO: PÁGINA ESP
-- ═══════════════════════════════════════════
local ESPPage = CreatePage("ESP")

CreateToggle(ESPPage, "Ativar ESP", false, function(state)
    Config.ESP.Enabled = state
end)
CreateToggle(ESPPage, "Caixas", true, function(state) Config.ESP.Boxes = state end)
CreateToggle(ESPPage, "Nomes", true, function(state) Config.ESP.Names = state end)
CreateToggle(ESPPage, "Tracers", false, function(state) Config.ESP.Tracers = state end)
CreateToggle(ESPPage, "Vida (Health)", true, function(state) Config.ESP.Health = state end)
CreateToggle(ESPPage, "Distância", true, function(state) Config.ESP.Distance = state end)
CreateToggle(ESPPage, "Team Check", false, function(state) Config.ESP.TeamCheck = state end)
CreateSlider(ESPPage, "Distância Máx", 100, 2000, 1000, function(v) Config.ESP.MaxDistance = v end)

-- ═══════════════════════════════════════════
-- CONTEÚDO: PÁGINA AIMBOT
-- ═══════════════════════════════════════════
local AimbotPage = CreatePage("Aimbot")

CreateToggle(AimbotPage, "Ativar Aimbot", false, function(state)
    Config.Aimbot.Enabled = state
end)
CreateToggle(AimbotPage, "Team Check", true, function(state) Config.Aimbot.TeamCheck = state end)
CreateToggle(AimbotPage, "Visible Check", false, function(state) Config.Aimbot.VisibleCheck = state end)
CreateSlider(AimbotPage, "FOV", 30, 500, 150, function(v) Config.Aimbot.FOV = v end)
CreateSlider(AimbotPage, "Smoothness (%)", 5, 100, 15, function(v)
    Config.Aimbot.Smoothness = v / 100
end)

-- ═══════════════════════════════════════════
-- CONTEÚDO: PÁGINA CONFIG
-- ═══════════════════════════════════════════
local ConfigPage = CreatePage("Config")

local InfoLabel = Instance.new("TextLabel")
InfoLabel.Size = UDim2.new(1, 0, 0, 60)
InfoLabel.BackgroundColor3 = Color3.fromRGB(26, 26, 34)
InfoLabel.BorderSizePixel = 0
InfoLabel.Text = "snzmods v1.0\nPainel universal\nDesenvolvido para Delta"
InfoLabel.TextColor3 = Color3.fromRGB(150, 150, 170)
InfoLabel.TextSize = 13
InfoLabel.Font = Enum.Font.Gotham
InfoLabel.Parent = ConfigPage

local InfoCorner = Instance.new("UICorner")
InfoCorner.CornerRadius = UDim.new(0, 10)
InfoCorner.Parent = InfoLabel

local ResetBtn = Instance.new("TextButton")
ResetBtn.Size = UDim2.new(1, 0, 0, 40)
ResetBtn.BackgroundColor3 = Color3.fromRGB(50, 30, 30)
ResetBtn.Text = "Reiniciar Configurações"
ResetBtn.TextColor3 = Color3.fromRGB(255, 180, 180)
ResetBtn.TextSize = 14
ResetBtn.Font = Enum.Font.GothamMedium
ResetBtn.Parent = ConfigPage

local ResetCorner = Instance.new("UICorner")
ResetCorner.CornerRadius = UDim.new(0, 10)
ResetCorner.Parent = ResetBtn

ResetBtn.MouseButton1Click:Connect(function()
    Config.ESP.Enabled = false
    Config.Aimbot.Enabled = false
    Config.ESP.MaxDistance = 1000
    Config.Aimbot.FOV = 150
    Config.Aimbot.Smoothness = 0.15
end)

-- ═══════════════════════════════════════════
-- NAVEGAÇÃO ENTRE PÁGINAS
-- ═══════════════════════════════════════════
local TabButtons = {}
local function AddTab(name)
    local btn = CreateTab(name)
    TabButtons[name] = btn

    btn.MouseButton1Click:Connect(function()
        for n, p in pairs(Pages) do
            p.Visible = (n == name)
        end
        for n, b in pairs(TabButtons) do
            if n == name then
                b.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
                b.TextColor3 = Color3.fromRGB(255, 255, 255)
            else
                b.BackgroundColor3 = Color3.fromRGB(28, 28, 38)
                b.TextColor3 = Color3.fromRGB(180, 180, 190)
            end
        end
    end)

    return btn
end

AddTab("ESP")
AddTab("Aimbot")
AddTab("Config")

-- Página inicial
Pages.ESP.Visible = true
TabButtons.ESP.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
TabButtons.ESP.TextColor3 = Color3.fromRGB(255, 255, 255)

-- ═══════════════════════════════════════════
-- BOTÕES: MINIMIZAR E FECHAR
-- ═══════════════════════════════════════════
local minimized = false
MinBtn.MouseButton1Click:Connect(function()
    minimized = not minimized
    Main:TweenSize(
        minimized and UDim2.new(0, 320, 0, 45) or UDim2.new(0, 320, 0, 420),
        Enum.EasingDirection.Out,
        Enum.EasingStyle.Quad,
        0.2,
        true
    )
    PageContainer.Visible = not minimized
    NavBar.Visible = not minimized
end)

CloseBtn.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- ═══════════════════════════════════════════
-- SISTEMA ESP
-- ═══════════════════════════════════════════
local ESPObjects = {}

local function createESP(player)
    if player == LocalPlayer then return end
    if ESPObjects[player] then return end

    local Box = Instance.new("Frame")
    Box.Name = player.Name
    Box.BackgroundTransparency = 1
    Box.BorderSizePixel = 0
    Box.Visible = false
    Box.Parent = ESPFolder

    local BoxStroke = Instance.new("UIStroke")
    BoxStroke.Color = Config.ESP.Color
    BoxStroke.Thickness = 1.5
    BoxStroke.Parent = Box

    local NameTag = Instance.new("TextLabel")
    NameTag.Name = "NameTag"
    NameTag.Size = UDim2.new(1, 0, 0, 16)
    NameTag.Position = UDim2.new(0, 0, 0, -20)
    NameTag.BackgroundTransparency = 1
    NameTag.Text = player.Name
    NameTag.TextColor3 = Color3.fromRGB(255, 255, 255)
    NameTag.TextSize = 12
    NameTag.Font = Enum.Font.GothamBold
    NameTag.TextStrokeTransparency = 0
    NameTag.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    NameTag.Parent = Box

    local DistTag = Instance.new("TextLabel")
    DistTag.Name = "DistTag"
    DistTag.Size = UDim2.new(1, 0, 0, 14)
    DistTag.Position = UDim2.new(0, 0, 1, 2)
    DistTag.BackgroundTransparency = 1
    DistTag.Text = ""
    DistTag.TextColor3 = Color3.fromRGB(200, 200, 200)
    DistTag.TextSize = 11
    DistTag.Font = Enum.Font.Gotham
    DistTag.TextStrokeTransparency = 0
    DistTag.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    DistTag.Parent = Box

    local HealthBar = Instance.new("Frame")
    HealthBar.Name = "HealthBar"
    HealthBar.Size = UDim2.new(0, 3, 1, 0)
    HealthBar.Position = UDim2.new(0, -7, 0, 0)
    HealthBar.BackgroundColor3 = Color3.fromRGB(0, 255, 100)
    HealthBar.BorderSizePixel = 0
    HealthBar.Parent = Box

    local HealthBg = Instance.new("Frame")
    HealthBg.Name = "HealthBg"
    HealthBg.Size = UDim2.new(0, 3, 1, 0)
    HealthBg.Position = UDim2.new(0, -7, 0, 0)
    HealthBg.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    HealthBg.BorderSizePixel = 0
    HealthBg.ZIndex = 0
    HealthBg.Parent = Box

    ESPObjects[player] = {
        Box = Box,
        Stroke = BoxStroke,
        NameTag = NameTag,
        DistTag = DistTag,
        HealthBar = HealthBar
    }
end

local function removeESP(player)
    if ESPObjects[player] then
        ESPObjects[player].Box:Destroy()
        ESPObjects[player] = nil
    end
end

Players.PlayerAdded:Connect(function(p)
    p.CharacterAdded:Connect(function()
        task.wait(0.5)
        createESP(p)
    end)
end)

Players.PlayerRemoving:Connect(removeESP)

for _, p in pairs(Players:GetPlayers()) do
    createESP(p)
end

-- ═══════════════════════════════════════════
-- LOOP DE RENDERIZAÇÃO ESP
-- ═══════════════════════════════════════════
local function isSameTeam(player)
    if not LocalPlayer.Team then return false end
    if not player.Team then return false end
    return LocalPlayer.Team == player.Team
end

local function isVisible(character)
    local head = character:FindFirstChild("Head")
    if not head then return false end
    local origin = Camera.CFrame.Position
    local dir = (head.Position - origin)
    local ray = Ray.new(origin, dir)
    local hit = Workspace:FindPartOnRayWithIgnoreList(ray, {LocalPlayer.Character, Camera})
    return hit == head
end

RunService.RenderStepped:Connect(function()
    if not Config.ESP.Enabled then
        for _, obj in pairs(ESPObjects) do
            obj.Box.Visible = false
        end
        return
    end

    for player, obj in pairs(ESPObjects) do
        local char = player.Character
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local head = char and char:FindFirstChild("Head")
        local humanoid = char and char:FindFirstChildOfClass("Humanoid")

        if not (char and hrp and head and humanoid and humanoid.Health > 0) then
            obj.Box.Visible = false
            continue
        end

        if Config.ESP.TeamCheck and isSameTeam(player) then
            obj.Box.Visible = false
            continue
        end

        local distance = (Camera.CFrame.Position - hrp.Position).Magnitude
        if distance > Config.ESP.MaxDistance then
            obj.Box.Visible = false
            continue
        end

        -- Obter pontos da caixa
        local topPos, topOnScreen = Camera:WorldToViewportPoint(head.Position + Vector3.new(0, 0.5, 0))
        local bottomPos, bottomOnScreen = Camera:WorldToViewportPoint(hrp.Position - Vector3.new(0, 3, 0))

        if topOnScreen and bottomOnScreen then
            local height = math.abs(topPos.Y - bottomPos.Y)
            local width = height * 0.55

            obj.Box.Size = UDim2.new(0, width, 0, height)
            obj.Box.Position = UDim2.new(0, topPos.X - width / 2, 0, topPos.Y)
            obj.Box.Visible = true

            -- Cor conforme distância
            local t = math.clamp(distance / Config.ESP.MaxDistance, 0, 1)
            obj.Stroke.Color = Color3.fromRGB(
                255 * t,
                170 * (1 - t) + 50,
                255 * (1 - t)
            )

            obj.NameTag.Visible = Config.ESP.Names
            obj.DistTag.Visible = Config.ESP.Distance
            obj.HealthBar.Visible = Config.ESP.Health

            if Config.ESP.Names then
                obj.NameTag.Text = player.Name
            end
            if Config.ESP.Distance then
                obj.DistTag.Text = string.format("%d studs", math.floor(distance))
            end
            if Config.ESP.Health then
                local hp = math.clamp(humanoid.Health / humanoid.MaxHealth, 0, 1)
                obj.HealthBar.Size = UDim2.new(0, 3, hp, 0)
                obj.HealthBar.BackgroundColor3 = Color3.fromRGB(
                    255 * (1 - hp),
                    255 * hp,
                    0
                )
            end
        else
            obj.Box.Visible = false
        end
    end
end)

-- ═══════════════════════════════════════════
-- SISTEMA AIMBOT
-- ═══════════════════════════════════════════
local aimbotActive = false

local function getClosestPlayer()
    local closest, shortest = nil, math.huge
    local mousePos = UserInputService:GetMouseLocation()

    for _, player in pairs(Players:GetPlayers()) do
        if player == LocalPlayer then continue end
        if Config.Aimbot.TeamCheck and isSameTeam(player) then continue end

        local char = player.Character
        local head = char and char:FindFirstChild("Head")
        if not head then continue end

        local hum = char:FindFirstChildOfClass("Humanoid")
        if not hum or hum.Health <= 0 then continue end

        if Config.Aimbot.VisibleCheck and not isVisible(char) then continue end

        local screenPos, onScreen = Camera:WorldToViewportPoint(head.Position)
        if not onScreen then continue end

        local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
        if dist < Config.Aimbot.FOV and dist < shortest then
            closest, shortest = head, dist
        end
    end

    return closest
end

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if not Config.Aimbot.Enabled then return end
    if input.UserInputType == Config.Aimbot.Key or input.KeyCode == Enum.KeyCode.ButtonL2 then
        aimbotActive = true
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Config.Aimbot.Key or input.KeyCode == Enum.KeyCode.ButtonL2 then
        aimbotActive = false
    end
end)

RunService.RenderStepped:Connect(function()
    if not (Config.Aimbot.Enabled and aimbotActive) then return end

    local target = getClosestPlayer()
    if not target then return end

    local camPos = Camera.CFrame.Position
    local targetDir = (target.Position - camPos).Unit
    local currentDir = Camera.CFrame.LookVector

    local newDir = currentDir:Lerp(targetDir, Config.Aimbot.Smoothness)
    Camera.CFrame = CFrame.new(camPos, camPos + newDir)
end)

-- ═══════════════════════════════════════════
-- NOTIFICAÇÃO INICIAL
-- ═══════════════════════════════════════════
local Notif = Instance.new("TextLabel")
Notif.Size = UDim2.new(0, 220, 0, 40)
Notif.Position = UDim2.new(0.5, -110, 0, 20)
Notif.BackgroundColor3 = Color3.fromRGB(18, 18, 24)
Notif.Text = "✔  snzmods carregado com sucesso"
Notif.TextColor3 = Color3.fromRGB(0, 200, 255)
Notif.TextSize = 13
Notif.Font = Enum.Font.GothamMedium
Notif.Parent = ScreenGui

local NCorner = Instance.new("UICorner")
NCorner.CornerRadius = UDim.new(0, 10)
NCorner.Parent = Notif

local NStroke = Instance.new("UIStroke")
NStroke.Color = Color3.fromRGB(0, 170, 255)
NStroke.Thickness = 1
NStroke.Parent = Notif

task.spawn(function()
    task.wait(3)
    for i = 1, 10 do
        Notif.BackgroundTransparency = i / 10
        Notif.TextTransparency = i / 10
        NStroke.Transparency = i / 10
        task.wait(0.03)
    end
    Notif:Destroy()
end)

print("[snzmods] Executado com sucesso!")
```
