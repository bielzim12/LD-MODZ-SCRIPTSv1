local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- // AVISO DE INICIALIZAÇÃO // --
Rayfield:Notify({
   Title = "LD MODZ V1",
   Content = "Bypass Ativado. Funções Corrigidas! Entre no Telegram.",
   Duration = 5,
   Image = 4483362458,
})

local Window = Rayfield:CreateWindow({
   Name = "LD MODZ V1",
   LoadingTitle = "Injetando LD MODZ V1...",
   LoadingSubtitle = "by LD MODZ",
   ConfigurationSaving = {Enabled = false},
   KeySystem = false 
})

-- // CONFIGS GLOBAIS // --
_G.Aimbot = false
_G.AimbotSmooth = 0.5 
_G.AimFOV = 100
_G.ShowFOV = false
_G.CameraFOV = 70 
_G.SpinBot = false 
_G.RGB_Weapons = false 
_G.Visual_Box = false
_G.Visual_Lines = false
_G.Visual_Health = false
_G.Visual_Names = false
_G.WalkSpeed = 16
_G.JumpPower = 50
_G.ThirdPerson = false
_G.ThirdPersonDistance = 15
_G.BlackSky = false

local Camera = workspace.CurrentCamera
local LP = game.Players.LocalPlayer
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Stats = game:GetService("Stats")

-- // INFO DISPLAY (FPS, PING, NOME) // --
local InfoDisplay = Drawing.new("Text")
InfoDisplay.Color = Color3.new(1, 1, 1)
InfoDisplay.Outline = true
InfoDisplay.Size = 18
InfoDisplay.Position = Vector2.new(10, 10)
InfoDisplay.Visible = true

-- // FOV CIRCLE // --
local FOVCircle = Drawing.new("Circle")
FOVCircle.Thickness = 1; FOVCircle.Color = Color3.fromRGB(255, 0, 0)
FOVCircle.Filled = false; FOVCircle.Visible = false

-- // ESP SYSTEM // --
local function AddESP(Player)
    local Box = Drawing.new("Square")
    local Line = Drawing.new("Line")
    local HealthBar = Drawing.new("Square")
    local NameTag = Drawing.new("Text")

    RunService:BindToRenderStep(Player.Name .. "_V43_ESP", 200, function()
        if Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") and Player.Character.Humanoid.Health > 0 then
            local Root = Player.Character.HumanoidRootPart
            local Pos, OnScreen = Camera:WorldToViewportPoint(Root.Position)
            if OnScreen then
                local Dist = (Camera.CFrame.Position - Root.Position).Magnitude
                local h = (Camera.ViewportSize.Y / Dist) * (50 / 70) * 7.2
                local w = h / 1.6
                local BoxPos = Vector2.new(Pos.X - w / 2, Pos.Y - h / 2)

                Box.Visible = _G.Visual_Box
                Box.Size = Vector2.new(w, h); Box.Position = BoxPos
                Box.Color = Color3.new(1, 1, 1)
                Box.Filled = false 

                Line.Visible = _G.Visual_Lines
                Line.From = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y)
                Line.To = Vector2.new(Pos.X, BoxPos.Y + h)

                if _G.Visual_Health then
                    local HP = Player.Character.Humanoid.Health / Player.Character.Humanoid.MaxHealth
                    HealthBar.Visible = true
                    HealthBar.Size = Vector2.new(2, h * HP)
                    HealthBar.Position = Vector2.new(BoxPos.X - 5, BoxPos.Y + (h * (1 - HP)))
                    HealthBar.Color = Color3.new(0, 1, 0); HealthBar.Filled = true
                else HealthBar.Visible = false end

                NameTag.Visible = _G.Visual_Names
                NameTag.Text = Player.Name:lower()
                NameTag.Position = Vector2.new(Pos.X, BoxPos.Y - 15)
                NameTag.Center = true; NameTag.Outline = true
            else
                for _, v in pairs({Box, Line, HealthBar, NameTag}) do v.Visible = false end
            end
        else
            for _, v in pairs({Box, Line, HealthBar, NameTag}) do v.Visible = false end
        end
    end)
end

for _, v in pairs(game.Players:GetPlayers()) do if v ~= LP then AddESP(v) end end
game.Players.PlayerAdded:Connect(function(v) if v ~= LP then AddESP(v) end end)

-- // INTERFACE // --
local TabCombat = Window:CreateTab("🎯 AIMBOT")
local TabVisual = Window:CreateTab("👁️ Visual")
local TabPlayer = Window:CreateTab("👤 Player")
local TabSettings = Window:CreateTab("⚙️ Configuração")

-- // AIMBOT TAB // --
TabCombat:CreateToggle({Name = "Aimbot", Callback = function(v) _G.Aimbot = v end}) 
TabCombat:CreateSlider({Name = "Suavização", Range = {0.1, 1}, Increment = 0.1, CurrentValue = 0.5, Callback = function(v) _G.AimbotSmooth = v end})
TabCombat:CreateSlider({Name = "AimFOV", Range = {0, 600}, Increment = 1, CurrentValue = 100, Callback = function(v) _G.AimFOV = v end})
TabCombat:CreateToggle({Name = "aimfov", Callback = function(v) _G.ShowFOV = v end})

-- // VISUAL // --
TabVisual:CreateToggle({Name = "esp box", Callback = function(v) _G.Visual_Box = v end})
TabVisual:CreateToggle({Name = "esp line", Callback = function(v) _G.Visual_Lines = v end})
TabVisual:CreateToggle({Name = "esp vida", Callback = function(v) _G.Visual_Health = v end})
TabVisual:CreateToggle({Name = "esp nome", Callback = function(v) _G.Visual_Names = v end})
TabVisual:CreateToggle({Name = "Céu Preto", Callback = function(v) _G.BlackSky = v end})
TabVisual:CreateSlider({Name = "Camera FOV", Range = {70, 120}, Increment = 1, CurrentValue = 70, Callback = function(v) _G.CameraFOV = v end})

-- // PLAYER // --
TabPlayer:CreateToggle({Name = "SpinBot", Callback = function(v) _G.SpinBot = v end})
TabPlayer:CreateToggle({Name = "Armas RGB", Callback = function(v) _G.RGB_Weapons = v end})
TabPlayer:CreateToggle({Name = "Terceira Pessoa", Callback = function(v) _G.ThirdPerson = v end})
TabPlayer:CreateSlider({Name = "Distância 3P", Range = {5, 100}, Increment = 1, CurrentValue = 15, Callback = function(v) _G.ThirdPersonDistance = v end})
TabPlayer:CreateSlider({Name = "Velocidade", Range = {16, 200}, Increment = 1, CurrentValue = 16, Callback = function(v) _G.WalkSpeed = v end})
TabPlayer:CreateSlider({Name = "Pulo", Range = {50, 300}, Increment = 1, CurrentValue = 50, Callback = function(v) _G.JumpPower = v end})

-- // CONFIGURAÇÃO // --
TabSettings:CreateButton({
    Name = "Liberar FPS",
    Callback = function() if setfpscap then setfpscap(400) end end
})

TabSettings:CreateButton({
    Name = "Anti-Lag (Tirar Texturas)",
    Callback = function() 
        for _, v in pairs(game:GetDescendants()) do
            if v:IsA("BasePart") then v.Material = "SmoothPlastic"; v.CastShadow = false
            elseif v:IsA("Decal") or v:IsA("Texture") then v:Destroy() 
            elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then v.Enabled = false end
        end
    end
})

TabSettings:CreateButton({
    Name = "Entrar no Telegram",
    Callback = function() 
        local link = "https://t.me/+4ad3yj-SqPc4NDYx"
        setclipboard(link)
        pcall(function() game:GetService("GuiService"):OpenBrowserWindow(link) end)
    end
})

-- // LOOP PRINCIPAL // --
RunService.RenderStepped:Connect(function()
    local fps = math.floor(1/RunService.RenderStepped:Wait())
    local ping = math.floor(Stats.Network.ServerStatsItem["Data Ping"]:GetValue())
    InfoDisplay.Text = string.format("FPS: %d | PING: %dms | USER: %s", fps, ping, LP.Name)

    Camera.FieldOfView = _G.CameraFOV
    local isShooting = UIS:IsMouseButtonPressed(Enum.UserInputType.MouseButton1) or UIS:IsMouseButtonPressed(Enum.UserInputType.Touch)

    if _G.BlackSky then
        game.Lighting.ClockTime = 0
        game.Lighting.Brightness = 2
        game.Lighting.OutdoorAmbient = Color3.new(0,0,0)
    end

    if _G.ThirdPerson then
        LP.CameraMode = Enum.CameraMode.Classic
        LP.CameraMaxZoomDistance = _G.ThirdPersonDistance
        LP.CameraMinZoomDistance = _G.ThirdPersonDistance
    else
        LP.CameraMaxZoomDistance = 12.8
        LP.CameraMinZoomDistance = 0.5
    end

    if LP.Character and LP.Character:FindFirstChild("Humanoid") then
        LP.Character.Humanoid.WalkSpeed = _G.WalkSpeed
        LP.Character.Humanoid.JumpPower = _G.JumpPower
        
        -- SpinBot com pausa para o dano contar
        if _G.SpinBot and not isShooting then
            LP.Character.HumanoidRootPart.CFrame = LP.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(60), 0)
        end

        -- RGB na Arma (Procurando em ferramentas e viewmodels)
        if _G.RGB_Weapons then
            local color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
            local function applyRGB(obj)
                for _, p in pairs(obj:GetDescendants()) do
                    if p:IsA("BasePart") or p:IsA("MeshPart") then
                        p.Color = color
                    elseif p:IsA("SpecialMesh") then
                        p.VertexColor = Vector3.new(color.R, color.G, color.B)
                    end
                end
            end
            applyRGB(LP.Character)
            applyRGB(Camera)
        end
    end

    FOVCircle.Visible = _G.ShowFOV
    FOVCircle.Radius = _G.AimFOV
    FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

    if _G.Aimbot and isShooting then
        local Target = nil
        local ShortestMag = _G.AimFOV 
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= LP and p.Character and p.Character:FindFirstChild("Head") and p.Character.Humanoid.Health > 0 then
                local ScreenPos, OnScreen = Camera:WorldToViewportPoint(p.Character.Head.Position)
                if OnScreen then
                    local RayParam = RaycastParams.new()
                    RayParam.FilterType = Enum.RaycastFilterType.Exclude
                    RayParam.FilterDescendantsInstances = {LP.Character, Camera}
                    local direction = (p.Character.Head.Position - Camera.CFrame.Position).Unit * (p.Character.Head.Position - Camera.CFrame.Position).Magnitude
                    local rayResult = workspace:Raycast(Camera.CFrame.Position, direction, RayParam)
                    if not rayResult or rayResult.Instance:IsDescendantOf(p.Character) then
                        local Mag = (Vector2.new(ScreenPos.X, ScreenPos.Y) - FOVCircle.Position).Magnitude
                        if Mag < ShortestMag then 
                            Target = p.Character.Head
                            ShortestMag = Mag
                        end
                    end
                end
            end
        end
        if Target then
            Camera.CFrame = Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position, Target.Position), _G.AimbotSmooth)
        end
    end
end)
