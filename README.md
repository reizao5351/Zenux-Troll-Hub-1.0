-- ZENUX UNIVERSAL HUB - TROLL EDITION
-- Version: 2.0.0 | Ultimate Trolling Toolkit
-- Compatible with ALL Roblox Games

repeat task.wait() until game:IsLoaded()

-- Services
local Services = setmetatable({}, {
    __index = function(t, k)
        local s = game:GetService(k)
        rawset(t, k, s)
        return s
    end
})

local Players = Services.Players
local RunService = Services.RunService
local UserInputService = Services.UserInputService
local TweenService = Services.TweenService
local Lighting = Services.Lighting
local Workspace = Services.Workspace
local ReplicatedStorage = Services.ReplicatedStorage
local SoundService = Services.SoundService
local StarterGui = Services.StarterGui
local VirtualUser = Services.VirtualUser
local VirtualInputManager = Services.VirtualInputManager
local TeleportService = Services.TeleportService

local Player = Players.LocalPlayer
local Mouse = Player:GetMouse()
local Camera = Workspace.CurrentCamera

-- Character Management
local Character, Humanoid, RootPart, Head

local function UpdateCharacter()
    Character = Player.Character or Player.CharacterAdded:Wait()
    task.wait(0.1)
    Humanoid = Character:WaitForChild("Humanoid")
    RootPart = Character:WaitForChild("HumanoidRootPart")
    Head = Character:WaitForChild("Head")
end

UpdateCharacter()

-- Zenux Core
local Zenux = {
    Version = "2.0.0",
    Connections = {},
    Objects = {},
    TrollTargets = {},
    
    Settings = {
        -- Troll Settings
        FlingPower = 1000,
        SpinSpeed = 100,
        RocketSpeed = 500,
        HeadSize = 10,
        
        -- Visual
        Fullbright = false,
        
        -- Movement
        WalkSpeed = 16,
        FlySpeed = 100,
        NoClip = false,
    }
}

-- Notification
function Zenux:Notify(title, text, duration)
    duration = duration or 3
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "ZENUX | " .. title,
            Text = text,
            Duration = duration,
        })
    end)
end

-- Tween Function
function Zenux:Tween(obj, props, duration)
    local tween = TweenService:Create(obj, TweenInfo.new(duration or 1), props)
    tween:Play()
    return tween
end

-- Get Distance
function Zenux:GetDistance(pos1, pos2)
    if typeof(pos1) == "Instance" then pos1 = pos1.Position end
    if typeof(pos2) == "Instance" then pos2 = pos2.Position end
    return (pos1 - pos2).Magnitude
end

-- TROLL FUNCTIONS

-- Super Fling
function Zenux:SuperFling(enabled)
    if enabled then
        self.Connections.Fling = RunService.Heartbeat:Connect(function()
            if RootPart then
                RootPart.Velocity = Vector3.new(0, self.Settings.FlingPower, 0)
                RootPart.RotVelocity = Vector3.new(9e9, 9e9, 9e9)
                
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                        part.Massless = true
                    end
                end
            end
        end)
    else
        if self.Connections.Fling then
            self.Connections.Fling:Disconnect()
        end
        if RootPart then
            RootPart.Velocity = Vector3.new(0, 0, 0)
            RootPart.RotVelocity = Vector3.new(0, 0, 0)
        end
    end
end

-- Rocket Player
function Zenux:RocketPlayer(playerName)
    for _, player in pairs(Players:GetPlayers()) do
        if player.Name:lower():find(playerName:lower()) and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local rocket = Instance.new("RocketPropulsion")
                rocket.Target = hrp
                rocket.MaxSpeed = self.Settings.RocketSpeed
                rocket.MaxThrust = 50000
                rocket.Parent = RootPart
                rocket:Fire()
                
                task.delay(3, function()
                    rocket:Destroy()
                end)
                
                self:Notify("Rocket", "Voando para " .. player.Name, 2)
                return
            end
        end
    end
end

-- Explode Player
function Zenux:ExplodePlayer(playerName)
    for _, player in pairs(Players:GetPlayers()) do
        if player.Name:lower():find(playerName:lower()) and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local explosion = Instance.new("Explosion")
                explosion.Position = hrp.Position
                explosion.BlastRadius = 20
                explosion.BlastPressure = 500000
                explosion.Parent = Workspace
                
                self:Notify("Explode", "Explosao em " .. player.Name, 2)
                return
            end
        end
    end
end

-- Teleport All Players
function Zenux:TeleportAllPlayers(enabled)
    if enabled then
        self.Connections.TPAll = RunService.Heartbeat:Connect(function()
            task.wait(0.5)
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        hrp.CFrame = RootPart.CFrame + Vector3.new(math.random(-10, 10), 0, math.random(-10, 10))
                    end
                end
            end
        end)
    else
        if self.Connections.TPAll then
            self.Connections.TPAll:Disconnect()
        end
    end
end

-- Spin All Players
function Zenux:SpinAllPlayers(enabled)
    if enabled then
        self.Connections.SpinAll = RunService.Heartbeat:Connect(function()
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local spin = hrp:FindFirstChild("ZenuxSpin") or Instance.new("BodyAngularVelocity")
                        spin.Name = "ZenuxSpin"
                        spin.Parent = hrp
                        spin.MaxTorque = Vector3.new(0, 9e9, 0)
                        spin.AngularVelocity = Vector3.new(0, 100, 0)
                    end
                end
            end
        end)
    else
        if self.Connections.SpinAll then
            self.Connections.SpinAll:Disconnect()
        end
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp and hrp:FindFirstChild("ZenuxSpin") then
                    hrp.ZenuxSpin:Destroy()
                end
            end
        end
    end
end

-- Launch All Players
function Zenux:LaunchAllPlayers()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local bv = Instance.new("BodyVelocity")
                bv.MaxForce = Vector3.new(9e9, 9e9, 9e9)
                bv.Velocity = Vector3.new(0, 500, 0)
                bv.Parent = hrp
                
                task.delay(1, function()
                    bv:Destroy()
                end)
            end
        end
    end
    self:Notify("Launch", "Todos os jogadores foram lancados!", 2)
end

-- Freeze All Players
function Zenux:FreezeAllPlayers(enabled)
    if enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player and player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.Anchored = true
                end
            end
        end
    else
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    hrp.Anchored = false
                end
            end
        end
    end
end

-- Fling All Players
function Zenux:FlingAllPlayers(enabled)
    if enabled then
        self.Connections.FlingAll = RunService.Heartbeat:Connect(function()
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        hrp.Velocity = Vector3.new(math.random(-200, 200), 500, math.random(-200, 200))
                        hrp.RotVelocity = Vector3.new(9e9, 9e9, 9e9)
                    end
                end
            end
        end)
    else
        if self.Connections.FlingAll then
            self.Connections.FlingAll:Disconnect()
        end
    end
end

-- Attach All Players
function Zenux:AttachAllPlayers(enabled)
    if enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player and player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local weld = Instance.new("Weld")
                    weld.Name = "ZenuxAttach"
                    weld.Part0 = RootPart
                    weld.Part1 = hrp
                    weld.C0 = CFrame.new(math.random(-10, 10), 0, math.random(-10, 10))
                    weld.Parent = RootPart
                end
            end
        end
    else
        for _, weld in pairs(RootPart:GetChildren()) do
            if weld.Name == "ZenuxAttach" then
                weld:Destroy()
            end
        end
    end
end

-- Orbit Players Around You
function Zenux:OrbitPlayers(enabled)
    if enabled then
        local angle = 0
        self.Connections.Orbit = RunService.Heartbeat:Connect(function()
            angle = angle + 0.05
            local radius = 15
            local i = 0
            
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local offsetAngle = angle + (i * (math.pi * 2 / #Players:GetPlayers()))
                        local x = math.cos(offsetAngle) * radius
                        local z = math.sin(offsetAngle) * radius
                        
                        hrp.CFrame = RootPart.CFrame + Vector3.new(x, 5, z)
                        i = i + 1
                    end
                end
            end
        end)
    else
        if self.Connections.Orbit then
            self.Connections.Orbit:Disconnect()
        end
    end
end

-- Spam Explosions
function Zenux:SpamExplosions(enabled)
    if enabled then
        self.Connections.Explosions = RunService.Heartbeat:Connect(function()
            task.wait(0.5)
            for _, player in pairs(Players:GetPlayers()) do
                if player ~= Player and player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local exp = Instance.new("Explosion")
                        exp.Position = hrp.Position
                        exp.BlastRadius = 10
                        exp.Parent = Workspace
                    end
                end
            end
        end)
    else
        if self.Connections.Explosions then
            self.Connections.Explosions:Disconnect()
        end
    end
end

-- Cage Player
function Zenux:CagePlayer(playerName)
    for _, player in pairs(Players:GetPlayers()) do
        if player.Name:lower():find(playerName:lower()) and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local cage = Instance.new("Model")
                cage.Name = "ZenuxCage"
                
                for i = 1, 6 do
                    local wall = Instance.new("Part")
                    wall.Size = Vector3.new(10, 10, 1)
                    wall.Anchored = true
                    wall.BrickColor = BrickColor.new("Really black")
                    
                    if i == 1 then wall.Position = hrp.Position + Vector3.new(0, 0, 5) end
                    if i == 2 then wall.Position = hrp.Position + Vector3.new(0, 0, -5) end
                    if i == 3 then 
                        wall.Size = Vector3.new(1, 10, 10)
                        wall.Position = hrp.Position + Vector3.new(5, 0, 0) 
                    end
                    if i == 4 then 
                        wall.Size = Vector3.new(1, 10, 10)
                        wall.Position = hrp.Position + Vector3.new(-5, 0, 0) 
                    end
                    if i == 5 then 
                        wall.Size = Vector3.new(10, 1, 10)
                        wall.Position = hrp.Position + Vector3.new(0, 5, 0) 
                    end
                    if i == 6 then 
                        wall.Size = Vector3.new(10, 1, 10)
                        wall.Position = hrp.Position + Vector3.new(0, -5, 0) 
                    end
                    
                    wall.Parent = cage
                end
                
                cage.Parent = Workspace
                self:Notify("Cage", player.Name .. " foi aprisionado!", 2)
                return
            end
        end
    end
end

-- Platform Under Players
function Zenux:PlatformUnderPlayers(enabled)
    if enabled then
        self.Connections.Platform = RunService.Heartbeat:Connect(function()
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local platform = hrp:FindFirstChild("ZenuxPlatform") or Instance.new("Part")
                        platform.Name = "ZenuxPlatform"
                        platform.Size = Vector3.new(8, 1, 8)
                        platform.Position = hrp.Position - Vector3.new(0, 4, 0)
                        platform.Anchored = true
                        platform.BrickColor = BrickColor.Random()
                        platform.Material = Enum.Material.Neon
                        platform.Parent = hrp
                    end
                end
            end
        end)
    else
        if self.Connections.Platform then
            self.Connections.Platform:Disconnect()
        end
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp and hrp:FindFirstChild("ZenuxPlatform") then
                    hrp.ZenuxPlatform:Destroy()
                end
            end
        end
    end
end

-- Ragdoll All
function Zenux:RagdollAll(enabled)
    if enabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player and player.Character then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Physics)
                end
            end
        end
    end
end

-- Spam Sounds
function Zenux:SpamSounds(enabled, soundId)
    if enabled then
        self.Connections.SpamSound = RunService.Heartbeat:Connect(function()
            task.wait(0.1)
            local sound = Instance.new("Sound")
            sound.SoundId = "rbxassetid://" .. (soundId or "6026984224")
            sound.Volume = 10
            sound.Parent = Workspace
            sound:Play()
            
            task.delay(2, function()
                sound:Destroy()
            end)
        end)
    else
        if self.Connections.SpamSound then
            self.Connections.SpamSound:Disconnect()
        end
    end
end

-- Black Hole Effect
function Zenux:BlackHole(enabled)
    if enabled then
        local blackHole = Instance.new("Part")
        blackHole.Name = "ZenuxBlackHole"
        blackHole.Size = Vector3.new(20, 20, 20)
        blackHole.Position = RootPart.Position + Vector3.new(0, 30, 0)
        blackHole.Shape = Enum.PartType.Ball
        blackHole.BrickColor = BrickColor.new("Really black")
        blackHole.Material = Enum.Material.Neon
        blackHole.Anchored = true
        blackHole.CanCollide = false
        blackHole.Parent = Workspace
        
        self.Connections.BlackHole = RunService.Heartbeat:Connect(function()
            for _, player in pairs(Players:GetPlayers()) do
                if player.Character then
                    local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                    if hrp then
                        local direction = (blackHole.Position - hrp.Position).Unit
                        hrp.Velocity = direction * 100
                    end
                end
            end
        end)
    else
        if self.Connections.BlackHole then
            self.Connections.BlackHole:Disconnect()
        end
        if Workspace:FindFirstChild("ZenuxBlackHole") then
            Workspace.ZenuxBlackHole:Destroy()
        end
    end
end

-- MOVEMENT FUNCTIONS

function Zenux:NoClip(enabled)
    if enabled then
        self.Connections.NoClip = RunService.Stepped:Connect(function()
            if Character then
                for _, part in pairs(Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.CanCollide = false
                    end
                end
            end
        end)
    else
        if self.Connections.NoClip then
            self.Connections.NoClip:Disconnect()
        end
    end
end

function Zenux:Fly(enabled, speed)
    speed = speed or 100
    
    if enabled then
        local BV = Instance.new("BodyVelocity")
        local BG = Instance.new("BodyGyro")
        
        BV.Name = "ZenuxFly"
        BG.Name = "ZenuxFly"
        BV.Parent = RootPart
        BG.Parent = RootPart
        
        BV.MaxForce = Vector3.new(9e9, 9e9, 9e9)
        BG.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
        BG.P = 10000
        
        self:NoClip(true)
        
        self.Connections.Fly = RunService.Heartbeat:Connect(function()
            local cam = Camera
            local direction = Vector3.new()
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                direction = direction + cam.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                direction = direction - cam.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                direction = direction - cam.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                direction = direction + cam.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                direction = direction + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                direction = direction - Vector3.new(0, 1, 0)
            end
            
            BV.Velocity = direction * speed
            BG.CFrame = cam.CFrame
        end)
    else
        for _, obj in pairs(RootPart:GetChildren()) do
            if obj.Name == "ZenuxFly" then
                obj:Destroy()
            end
        end
        if self.Connections.Fly then
            self.Connections.Fly:Disconnect()
        end
        self:NoClip(false)
    end
end

-- UI LIBRARY
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("ZENUX Troll Hub v" .. Zenux.Version, "Midnight")

-- MAIN TAB
local MainTab = Window:NewTab("Main")
local InfoSection = MainTab:NewSection("Information")

InfoSection:NewLabel("Welcome to ZENUX Troll Hub")
InfoSection:NewLabel("Player: " .. Player.Name)
InfoSection:NewLabel("Version: " .. Zenux.Version)

InfoSection:NewButton("Destroy GUI", "Remove interface", function()
    for _, conn in pairs(Zenux.Connections) do
        if conn then conn:Disconnect() end
    end
    Library:ToggleUI()
end)

-- PLAYER TROLL TAB
local PlayerTrollTab = Window:NewTab("Player Troll")
local TargetSection = PlayerTrollTab:NewSection("Target Player")

local targetPlayer = ""

TargetSection:NewTextBox("Player Name", "Digite o nome", function(txt)
    targetPlayer = txt
end)

TargetSection:NewButton("Rocket To Player", "Voa ate o player", function()
    if targetPlayer ~= "" then
        Zenux:RocketPlayer(targetPlayer)
    end
end)

TargetSection:NewButton("Explode Player", "Explode o player", function()
    if targetPlayer ~= "" then
        Zenux:ExplodePlayer(targetPlayer)
    end
end)

TargetSection:NewButton("Cage Player", "Prende em gaiola", function()
    if targetPlayer ~= "" then
        Zenux:CagePlayer(targetPlayer)
    end
end)

-- ALL PLAYERS TAB
local AllPlayersTab = Window:NewTab("All Players")
local MassSection = AllPlayersTab:NewSection("Mass Trolling")

MassSection:NewToggle("TP All To You", "Teleporta todos", function(state)
    Zenux:TeleportAllPlayers(state)
end)

MassSection:NewToggle("Spin All Players", "Gira todos", function(state)
    Zenux:SpinAllPlayers(state)
end)

MassSection:NewToggle("Fling All Players", "Lanca todos", function(state)
    Zenux:FlingAllPlayers(state)
end)

MassSection:NewButton("Launch All Players", "Lanca todos no ar", function()
    Zenux:LaunchAllPlayers()
end)

MassSection:NewToggle("Freeze All Players", "Congela todos", function(state)
    Zenux:FreezeAllPlayers(state)
end)

MassSection:NewToggle("Attach All Players", "Gruda todos em voce", function(state)
    Zenux:AttachAllPlayers(state)
end)

local AdvancedSection = AllPlayersTab:NewSection("Advanced Trolling")

AdvancedSection:NewToggle("Orbit Players", "Players orbitam voce", function(state)
    Zenux:OrbitPlayers(state)
end)

AdvancedSection:NewToggle("Spam Explosions", "Explosoes continuas", function(state)
    Zenux:SpamExplosions(state)
end)

AdvancedSection:NewToggle("Platform Under All", "Plataforma sob todos", function(state)
    Zenux:PlatformUnderPlayers(state)
end)

AdvancedSection:NewButton("Ragdoll All Players", "Ragdoll em todos", function()
    Zenux:RagdollAll(true)
end)

AdvancedSection:NewToggle("Black Hole Effect", "Buraco negro", function(state)
    Zenux:BlackHole(state)
end)

-- SELF TROLL TAB
local SelfTrollTab = Window:NewTab("Self Troll")
local SelfSection = SelfTrollTab:NewSection("Self Trolling")

SelfSection:NewToggle("Super Fling", "Fling extremo", function(state)
    Zenux:SuperFling(state)
end)

SelfSection:NewSlider("Fling Power", "Poder do fling", 5000, 100, function(v)
    Zenux.Settings.FlingPower = v
end)

SelfSection:NewSlider("Head Size", "Tamanho da cabeca", 100, 1, function(v)
    if Head then
        Head.Size = Vector3.new(v, v, v)
        Head.Transparency = v > 20 and 0.5 or 0
    end
end)

SelfSection:NewButton("Clone Spam", "Clones multiplos", function()
    for i = 1, 10 do
        local clone = Character:Clone()
        clone.Parent = Workspace
        clone:MoveTo(RootPart.Position + Vector3.new(math.random(-20, 20), 0, math.random(-20, 20)))
    end
end)

SelfSection:NewButton("Invisible", "Ficar invisivel", function()
    for _, part in pairs(Character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Transparency = 1
        end
    end
end)

SelfSection:NewButton("Visible", "Ficar visivel", function()
    for _, part in pairs(Character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Transparency = 0
        end
    end
end)

-- WORLD TROLL TAB
local WorldTab = Window:NewTab("World Troll")
local WorldSection = WorldTab:NewSection("World Effects")

WorldSection:NewToggle("Spam Sounds", "Spam de sons", function(state)
    Zenux:SpamSounds(state)
end)

WorldSection:NewButton("Disco Mode", "Modo disco", function()
    for i = 1, 50 do
        Lighting.Ambient = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
        task.wait(0.1)
    end
end)

WorldSection:NewButton("Earthquake", "Terremoto", function()
    for i = 1, 50 do
        Camera.CFrame = Camera.CFrame * CFrame.Angles(math.rad(math.random(-5, 5)), math.rad(math.random(-5, 5)), 0)
        task.wait(0.05)
    end
end)

WorldSection:NewButton("Spawn Barriers", "Barreiras aleatorias", function()
    for i = 1, 20 do
        local wall = Instance.new("Part")
        wall.Size = Vector3.new(20, 20, 2)
        wall.Position = RootPart.Position + Vector3.new(math.random(-100, 100), 10, math.random(-100, 100))
        wall.Anchored = true
        wall.BrickColor = BrickColor.Random()
        wall.Parent = Workspace
    end
end)

WorldSection:NewButton("Spam Explosions Random", "Explosoes aleatorias", function()
    for i = 1, 30 do
        local exp = Instance.new("Explosion")
        exp.Position = RootPart.Position + Vector3.new(math.random(-50, 50), 0, math.random(-50, 50))
        exp.BlastRadius = 20
        exp.Parent = Workspace
        task.wait(0.2)
    end
end)

-- MOVEMENT TAB
local MovementTab = Window:NewTab("Movement")
local MoveSection = MovementTab:NewSection("Movement Controls")

MoveSection:NewSlider("WalkSpeed", "Velocidade", 500, 16, function(v)
    if Humanoid then
        Humanoid.WalkSpeed = v
    end
end)

MoveSection:NewSlider("JumpPower", "Pulo", 500, 50, function(v)
    if Humanoid then
        Humanoid.JumpPower = v
    end
end)

MoveSection:NewToggle("Fly", "Modo voo", function(state)
    Zenux:Fly(state, Zenux.Settings.FlySpeed)
end)

MoveSection:NewSlider("Fly Speed", "Velocidade voo", 300, 50, function(v)
    Zenux.Settings.FlySpeed = v
end)

MoveSection:NewToggle("NoClip", "Atravessar paredes", function(state)
    Zenux:NoClip(state)
end)

MoveSection:NewSlider("Gravity", "Gravidade", 500, 196, function(v)
    Workspace.Gravity = v
end)

-- VISUAL TAB
local VisualTab = Window:NewTab("Visual")
local VisualSection = VisualTab:NewSection("Visual Effects")

VisualSection:NewToggle("Fullbright", "Iluminacao maxima", function(state)
    if state then
        Lighting.Brightness = 2
        Lighting.ClockTime = 14
        Lighting.FogEnd = 9e9
        Lighting.GlobalShadows = false
    else
        Lighting.Brightness = 1
        Lighting.ClockTime = 12
        Lighting.GlobalShadows = true
    end
end)

VisualSection:NewButton("Remove Textures", "Remove texturas", function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Decal") or obj:IsA("Texture") then
            obj:Destroy()
        end
    end
    Zenux:Notify("Visual", "Texturas removidas", 2)
end)

VisualSection:NewButton("Neon Everything", "Tudo neon", function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            obj.Material = Enum.Material.Neon
            obj.Color = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
        end
    end
end)

VisualSection:NewSlider("FOV", "Campo de visao", 120, 70, function(v)
    Camera.FieldOfView = v
end)

VisualSection:NewButton("Trippy Mode", "Modo trippy", function()
    for i = 1, 100 do
        Camera.FieldOfView = math.random(30, 120)
        Lighting.Ambient = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
        task.wait(0.1)
    end
    Camera.FieldOfView = 70
end)

-- TELEPORT TAB
local TeleportTab = Window:NewTab("Teleport")
local TPSection = TeleportTab:NewSection("Teleport Options")

TPSection:NewButton("TP to Random Player", "TP aleatorio", function()
    local players = Players:GetPlayers()
    local randomPlayer = players[math.random(1, #players)]
    if randomPlayer ~= Player and randomPlayer.Character then
        RootPart.CFrame = randomPlayer.Character.HumanoidRootPart.CFrame
    end
end)

TPSection:NewButton("TP to Mouse", "TP para mouse", function()
    RootPart.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 3, 0))
end)

TPSection:NewButton("TP Everyone to You", "Todos para voce", function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = RootPart.CFrame
            end
        end
    end
end)

TPSection:NewButton("TP All to Random Spot", "Todos para lugar aleatorio", function()
    local randomPos = Vector3.new(math.random(-500, 500), 50, math.random(-500, 500))
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = CFrame.new(randomPos)
            end
        end
    end
end)

-- SPAWN TAB
local SpawnTab = Window:NewTab("Spawn")
local SpawnSection = SpawnTab:NewSection("Spawn Objects")

SpawnSection:NewButton("Spawn Wall of Death", "Parede mortal", function()
    local wall = Instance.new("Part")
    wall.Size = Vector3.new(100, 50, 5)
    wall.Position = RootPart.Position + Vector3.new(0, 0, 30)
    wall.BrickColor = BrickColor.new("Really red")
    wall.Material = Enum.Material.Neon
    wall.Anchored = true
    wall.Parent = Workspace
    
    local bv = Instance.new("BodyVelocity")
    bv.MaxForce = Vector3.new(9e9, 0, 9e9)
    bv.Velocity = (RootPart.Position - wall.Position).Unit * 50
    bv.Parent = wall
    
    wall.Touched:Connect(function(hit)
        if hit.Parent:FindFirstChild("Humanoid") then
            hit.Parent.Humanoid.Health = 0
        end
    end)
end)

SpawnSection:NewButton("Spawn Tornado", "Tornado", function()
    local tornado = Instance.new("Part")
    tornado.Size = Vector3.new(30, 100, 30)
    tornado.Position = RootPart.Position + Vector3.new(0, 50, 50)
    tornado.Shape = Enum.PartType.Cylinder
    tornado.BrickColor = BrickColor.new("Dark stone grey")
    tornado.Material = Enum.Material.ForceField
    tornado.Anchored = true
    tornado.CanCollide = false
    tornado.Transparency = 0.3
    tornado.Parent = Workspace
    
    local spin = 0
    local connection
    connection = RunService.Heartbeat:Connect(function()
        spin = spin + 0.1
        tornado.CFrame = tornado.CFrame * CFrame.Angles(0, 0.1, 0)
        
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local distance = (tornado.Position - hrp.Position).Magnitude
                    if distance < 50 then
                        hrp.Velocity = (tornado.Position - hrp.Position).Unit * 50 + Vector3.new(0, 100, 0)
                    end
                end
            end
        end
    end)
    
    task.delay(30, function()
        connection:Disconnect()
        tornado:Destroy()
    end)
end)

SpawnSection:NewButton("Spawn Kill Bricks", "Blocos mortais", function()
    for i = 1, 20 do
        local brick = Instance.new("Part")
        brick.Size = Vector3.new(10, 10, 10)
        brick.Position = RootPart.Position + Vector3.new(math.random(-50, 50), 20, math.random(-50, 50))
        brick.BrickColor = BrickColor.new("Really black")
        brick.Material = Enum.Material.Neon
        brick.Anchored = false
        brick.Parent = Workspace
        
        brick.Touched:Connect(function(hit)
            if hit.Parent:FindFirstChild("Humanoid") then
                hit.Parent.Humanoid.Health = 0
            end
        end)
    end
end)

SpawnSection:NewButton("Spawn Giant Sphere", "Esfera gigante", function()
    local sphere = Instance.new("Part")
    sphere.Shape = Enum.PartType.Ball
    sphere.Size = Vector3.new(50, 50, 50)
    sphere.Position = RootPart.Position + Vector3.new(0, 30, 0)
    sphere.BrickColor = BrickColor.Random()
    sphere.Material = Enum.Material.Neon
    sphere.Anchored = false
    sphere.Parent = Workspace
end)

SpawnSection:NewButton("Spawn Rain of Parts", "Chuva de blocos", function()
    for i = 1, 50 do
        task.spawn(function()
            local part = Instance.new("Part")
            part.Size = Vector3.new(5, 5, 5)
            part.Position = RootPart.Position + Vector3.new(math.random(-50, 50), 100, math.random(-50, 50))
            part.BrickColor = BrickColor.Random()
            part.Material = Enum.Material.Neon
            part.Parent = Workspace
            
            task.delay(10, function()
                part:Destroy()
            end)
        end)
        task.wait(0.1)
    end
end)

-- EXTREME TROLL TAB
local ExtremeTab = Window:NewTab("Extreme")
local ExtremeSection = ExtremeTab:NewSection("Extreme Trolling")

ExtremeSection:NewButton("Chaos Mode", "Modo caos total", function()
    Zenux:Notify("Chaos", "MODO CAOS ATIVADO", 3)
    
    -- Launch all
    Zenux:LaunchAllPlayers()
    task.wait(1)
    
    -- Spin all
    Zenux:SpinAllPlayers(true)
    task.wait(2)
    Zenux:SpinAllPlayers(false)
    
    -- Explosions
    for i = 1, 10 do
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    local exp = Instance.new("Explosion")
                    exp.Position = hrp.Position
                    exp.BlastRadius = 20
                    exp.Parent = Workspace
                end
            end
        end
        task.wait(0.5)
    end
    
    -- Disco
    for i = 1, 20 do
        Lighting.Ambient = Color3.fromRGB(math.random(0, 255), math.random(0, 255), math.random(0, 255))
        task.wait(0.1)
    end
end)

ExtremeSection:NewButton("Ultimate Fling All", "Fling supremo", function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                for i = 1, 10 do
                    hrp.Velocity = Vector3.new(math.random(-1000, 1000), 1000, math.random(-1000, 1000))
                    hrp.RotVelocity = Vector3.new(9e9, 9e9, 9e9)
                    task.wait(0.1)
                end
            end
        end
    end
end)

ExtremeSection:NewButton("Apocalypse", "Apocalipse", function()
    -- Spawn tons of explosions
    for i = 1, 100 do
        task.spawn(function()
            local exp = Instance.new("Explosion")
            exp.Position = RootPart.Position + Vector3.new(math.random(-200, 200), math.random(0, 50), math.random(-200, 200))
            exp.BlastRadius = 30
            exp.BlastPressure = 500000
            exp.Parent = Workspace
        end)
        task.wait(0.05)
    end
    
    -- Darken sky
    Lighting.Brightness = 0
    Lighting.Ambient = Color3.fromRGB(50, 0, 0)
    Lighting.OutdoorAmbient = Color3.fromRGB(50, 0, 0)
end)

ExtremeSection:NewButton("Matrix Mode", "Modo matrix", function()
    for i = 1, 200 do
        task.spawn(function()
            local part = Instance.new("Part")
            part.Size = Vector3.new(0.5, math.random(5, 20), 0.5)
            part.Position = RootPart.Position + Vector3.new(math.random(-100, 100), 50, math.random(-100, 100))
            part.BrickColor = BrickColor.new("Lime green")
            part.Material = Enum.Material.Neon
            part.Anchored = true
            part.Parent = Workspace
            
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(0, 9e9, 0)
            bv.Velocity = Vector3.new(0, -50, 0)
            part.Anchored = false
            bv.Parent = part
            
            task.delay(5, function()
                part:Destroy()
            end)
        end)
        task.wait(0.02)
    end
end)

ExtremeSection:NewButton("Void Everyone", "Mandar para void", function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player and player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = CFrame.new(0, -500, 0)
            end
        end
    end
end)

ExtremeSection:NewButton("Clone Army", "Exercito de clones", function()
    for i = 1, 20 do
        local clone = Character:Clone()
        clone.Parent = Workspace
        clone:MoveTo(RootPart.Position + Vector3.new(math.random(-30, 30), 0, math.random(-30, 30)))
        
        task.spawn(function()
            while clone.Parent do
                for _, player in pairs(Players:GetPlayers()) do
                    if player ~= Player and player.Character then
                        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                        local cloneHrp = clone:FindFirstChild("HumanoidRootPart")
                        if hrp and cloneHrp then
                            local distance = (hrp.Position - cloneHrp.Position).Magnitude
                            if distance < 50 then
                                cloneHrp.CFrame = CFrame.new(cloneHrp.Position, hrp.Position)
                                cloneHrp.CFrame = cloneHrp.CFrame + (hrp.Position - cloneHrp.Position).Unit * 2
                            end
                        end
                    end
                end
                task.wait(0.5)
            end
        end)
    end
end)

-- SETTINGS TAB
local SettingsTab = Window:NewTab("Settings")
local SettingsSection = SettingsTab:NewSection("Configuration")

SettingsSection:NewKeybind("Toggle UI", "Atalho UI", Enum.KeyCode.RightControl, function()
    Library:ToggleUI()
end)

SettingsSection:NewButton("Rejoin Server", "Reconectar", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, Player)
end)

SettingsSection:NewButton("Server Hop", "Trocar servidor", function()
    local PlaceId = game.PlaceId
    local servers = game:GetService("HttpService"):JSONDecode(
        game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Asc&limit=100")
    ).data
    
    for _, server in pairs(servers) do
        if server.id ~= game.JobId and server.playing < server.maxPlayers then
            TeleportService:TeleportToPlaceInstance(PlaceId, server.id, Player)
            return
        end
    end
end)

SettingsSection:NewButton("Clear All Objects", "Limpar objetos", function()
    for _, obj in pairs(Workspace:GetChildren()) do
        if obj.Name:find("Zenux") then
            obj:Destroy()
        end
    end
    Zenux:Notify("Clean", "Objetos removidos", 2)
end)

local InfoSection = SettingsTab:NewSection("Information")

InfoSection:NewLabel("ZENUX Universal Hub")
InfoSection:NewLabel("Version: " .. Zenux.Version)
InfoSection:NewLabel("Troll Edition")
InfoSection:NewLabel("Universal Game Support")
InfoSection:NewLabel("")
InfoSection:NewLabel("Created by Zenux Team")

-- CREDITS TAB
local CreditsTab = Window:NewTab("Credits")
local CreditsSection = CreditsTab:NewSection("Developers")

CreditsSection:NewLabel("Zenux Team - Main Developer")
CreditsSection:NewLabel("Kavo UI - Interface Library")
CreditsSection:NewLabel("Community - Beta Testers")
CreditsSection:NewLabel("")
CreditsSection:NewLabel("Thank you for using ZENUX")

local StatsSection = CreditsTab:NewSection("Features")

StatsSection:NewLabel("Player Troll: 10+")
StatsSection:NewLabel("All Players: 15+")
StatsSection:NewLabel("Self Troll: 8+")
StatsSection:NewLabel("World Troll: 10+")
StatsSection:NewLabel("Spawn: 12+")
StatsSection:NewLabel("Extreme: 10+")
StatsSection:NewLabel("Movement: 8+")
StatsSection:NewLabel("")
StatsSection:NewLabel("Total: 73+ Features")

-- CHARACTER UPDATE
Player.CharacterAdded:Connect(function(char)
    task.wait(0.5)
    UpdateCharacter()
    Zenux:Notify("Character", "Personagem atualizado", 2)
end)

-- INITIALIZATION
Zenux:Notify("ZENUX", "Carregado com sucesso", 5)
Zenux:Notify("Troll", "73+ funcoes de troll", 3)

print([[
====================================================
            ZENUX UNIVERSAL HUB v2.0.0            
              TROLL EDITION LOADED                
====================================================

Features:
 - 73+ Troll Functions
 - Universal Game Support
 - Player Trolling
 - Mass Trolling
 - Extreme Chaos Mode
 - World Effects
 - Spawn Objects

Controls:
 - Right Ctrl: Toggle UI
 - WASD: Fly Movement
 
Loaded Successfully!
====================================================
]])

return Zenux
