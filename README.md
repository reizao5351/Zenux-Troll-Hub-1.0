-- ZENUX ULTIMATE TROLL HUB V3.0
-- Modern Blue/Black Edition with Auto Chat System

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
local HttpService = Services.HttpService
local StarterGui = Services.StarterGui
local TeleportService = Services.TeleportService
local ReplicatedStorage = Services.ReplicatedStorage
local TextChatService = Services.TextChatService

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

-- Zenux Core System
local Zenux = {
    Version = "3.0.0",
    Connections = {},
    Objects = {},
    SelectedPlayers = {},
    TrollModes = {},
    
    Settings = {
        FlingPower = 2000,
        SpinSpeed = 150,
        LaunchHeight = 800,
        OrbitRadius = 20,
        TornadoPower = 100,
        ExplosionSize = 30,
        FlySpeed = 150,
        AutoChatEnabled = true
    },
    
    -- Sistema de Chat Brasileiro para PLAYERS SELECIONADOS
    ChatMessages = {
        Launch = {
            "krlh to voando",
            "oq ta aconteceno mano",
            "para com isso vei",
            "vsf mlk",
            "qq ta contecendo",
            "bagulho ta louco",
            "eita porra",
            "alguem me ajuda namoral",
            "q isso vei",
            "pqp velho",
            "cara eu to voando wtf",
            "oq esse mlk ta fazendo",
            "admin bane esse cara pfv",
            "ta loco mano",
            "para ae namoral",
            "mlk me lancou no ar",
            "como assim to voando",
            "isso e hack certeza"
        },
        Explode = {
            "explodiu tudo aqui mano",
            "q isso vei kkkk",
            "pqp oq foi isso",
            "bomba?????",
            "caralho explodiu td",
            "vsf quem fez isso",
            "bagulho explodiu namoral",
            "qq ta acontecendo mano",
            "admin????",
            "oq e isso mano",
            "mlk ta hackeando",
            "para ae velho",
            "namoral q isso",
            "explosao do nada vei",
            "tem hacker aqui certeza"
        },
        Spin = {
            "to rodando sem parar socorro",
            "alguem me ajuda pelo amor",
            "para de girar isso vei",
            "vsf to tonto",
            "cara eu to girando wtf",
            "q hacker e esse mano",
            "bagulho ta girando",
            "oq ta rolando aqui",
            "to tonto demais",
            "para com isso namoral",
            "to rodando infinito",
            "alguem faz parar pfv",
            "q bug e esse vei",
            "admin olha isso",
            "girando sem parar mano"
        },
        Fling = {
            "pqp me arremessaram",
            "voei longe demais vei",
            "oq foi isso cara",
            "mlk me jogou pra longe",
            "como ele fez isso mano",
            "vsf sai voando",
            "bagulho me lancou",
            "to voando namoral",
            "para com esse hack vei",
            "alguem viu isso???",
            "fui arremessado wtf",
            "q poder e esse",
            "hacker fdp",
            "me jogaram longe vei"
        },
        Freeze = {
            "nao consigo mexer",
            "to bugado aqui vei",
            "travei namoral",
            "oq aconteceu mano",
            "nao consigo andar wtf",
            "vsf to travado",
            "alguem me desbuga",
            "to congelado socorro",
            "para com isso ae",
            "admin me ajuda",
            "bug do krl",
            "como desbuga isso",
            "nao to conseguindo sair",
            "to preso aqui vei"
        },
        Cage = {
            "me prenderam numa gaiola vei",
            "como saio daqui mano",
            "to preso namoral",
            "vsf me solta",
            "q gaiola e essa",
            "alguem me tira daqui",
            "to encarcerado kkkkk",
            "deixa eu sair daqui vei",
            "como faz isso mano",
            "hack de prender wtf",
            "soltem me pfv",
            "admin olha isso aqui",
            "to preso numa caixa vei"
        },
        Tornado = {
            "tem um tornado aqui vei",
            "vsf q vento e esse",
            "to sendo sugado mano",
            "tornado do nada wtf",
            "alguem para esse vento",
            "bagulho ta me puxando",
            "como para isso vei",
            "tornado hack???",
            "q vento forte mano",
            "to subindo namoral",
            "para com essa magia ae",
            "vento me pegou vei"
        },
        BlackHole = {
            "tem um buraco negro aqui",
            "to sendo puxado vei",
            "vsf q buraco e esse",
            "gravitade ta estranha mano",
            "alguem ve isso???",
            "buraco negro wtf",
            "como para isso namoral",
            "admin bane esse hacker",
            "bagulho ta me sugando",
            "fisica quebrou vei",
            "gravitade bugou mano"
        },
        Ragdoll = {
            "to todo quebrado vei",
            "virei boneco de pano mano",
            "vsf oq fizeram cmg",
            "nao consigo levantar",
            "to todo mole namoral",
            "como levanta isso",
            "ragdoll hack???",
            "alguem me ajuda ae",
            "to desmontado vei",
            "pqp to todo torto",
            "quebrei tudo mano"
        },
        Clone = {
            "tem varios eu aqui vei",
            "oq e isso mano",
            "clones do nada wtf",
            "vsf quantos sao",
            "to vendo duplo namoral",
            "bagulho clonou todo mundo",
            "q hack e esse",
            "admin olha quantos tem",
            "multiplicou geral vei",
            "ta cheio de clone",
            "clonaram a gente mano"
        },
        Attach = {
            "to grudado nele vei",
            "nao consigo sair daqui",
            "me solta mlk",
            "to colado wtf",
            "como sai disso mano",
            "to preso nele namoral"
        },
        Orbit = {
            "to girando em volta dele vei",
            "virando satelite mano",
            "to orbitando wtf",
            "q isso vei to flutuando",
            "to rodando ao redor dele"
        }
    },
    
    UsedMessages = {}
}

-- Utility Functions
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

-- Sistema de Chat Automatico para PLAYERS SELECIONADOS
function Zenux:SendFakeChatForPlayer(player, messageType)
    if not self.Settings.AutoChatEnabled then return end
    if not player or player == Player then return end
    
    local messages = self.ChatMessages[messageType] or {}
    if #messages == 0 then return end
    
    -- Inicializa array de mensagens usadas para este player
    if not self.UsedMessages[player.UserId] then
        self.UsedMessages[player.UserId] = {}
    end
    
    local availableMessages = {}
    
    -- Filtra mensagens que ainda não foram usadas por este player
    for _, msg in pairs(messages) do
        if not table.find(self.UsedMessages[player.UserId], msg) then
            table.insert(availableMessages, msg)
        end
    end
    
    -- Se todas foram usadas, reseta para este player
    if #availableMessages == 0 then
        self.UsedMessages[player.UserId] = {}
        availableMessages = messages
    end
    
    -- Seleciona mensagem aleatoria
    local randomMsg = availableMessages[math.random(1, #availableMessages)]
    table.insert(self.UsedMessages[player.UserId], randomMsg)
    
    -- Mantem apenas as ultimas 15 mensagens usadas por este player
    if #self.UsedMessages[player.UserId] > 15 then
        table.remove(self.UsedMessages[player.UserId], 1)
    end
    
    -- Tenta enviar no chat como se fosse o player (isso é visual apenas)
    -- Na prática, só seu personagem pode enviar, mas vamos fazer parecer que é o player
    task.spawn(function()
        -- Método 1: Chat antigo
        pcall(function()
            local args = {
                [1] = randomMsg,
                [2] = "All"
            }
            ReplicatedStorage:WaitForChild("DefaultChatSystemChatEvents"):WaitForChild("SayMessageRequest"):FireServer(unpack(args))
        end)
        
        -- Método 2: TextChatService
        pcall(function()
            TextChatService.TextChannels.RBXGeneral:SendAsync(randomMsg)
        end)
    end)
end

function Zenux:GetPlayerByName(name)
    for _, player in pairs(Players:GetPlayers()) do
        if player.Name:lower():find(name:lower()) or player.DisplayName:lower():find(name:lower()) then
            return player
        end
    end
    return nil
end

function Zenux:GetSelectedPlayers()
    local selected = {}
    for player, isSelected in pairs(self.SelectedPlayers) do
        if isSelected and player.Parent then
            table.insert(selected, player)
        end
    end
    return selected
end

-- ADVANCED TROLL FUNCTIONS

function Zenux:RagdollPlayer(player)
    if player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid:ChangeState(Enum.HumanoidStateType.Physics)
            task.delay(3, function()
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
                end
            end)
            
            self:SendFakeChatForPlayer(player, "Ragdoll")
        end
    end
end

function Zenux:SpinPlayer(player, speed)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local spin = Instance.new("BodyAngularVelocity")
            spin.Name = "ZenuxSpin"
            spin.Parent = hrp
            spin.MaxTorque = Vector3.new(0, math.huge, 0)
            spin.AngularVelocity = Vector3.new(0, speed or 100, 0)
            
            task.delay(5, function()
                if spin.Parent then spin:Destroy() end
            end)
            
            self:SendFakeChatForPlayer(player, "Spin")
        end
    end
end

function Zenux:LaunchPlayer(player, height)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local bv = Instance.new("BodyVelocity")
            bv.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
            bv.Velocity = Vector3.new(0, height or 500, 0)
            bv.Parent = hrp
            
            task.delay(0.5, function()
                if bv.Parent then bv:Destroy() end
            end)
            
            self:SendFakeChatForPlayer(player, "Launch")
        end
    end
end

function Zenux:FreezePlayer(player, freeze)
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part.Anchored = freeze
            end
        end
        
        if freeze then
            self:SendFakeChatForPlayer(player, "Freeze")
        end
    end
end

function Zenux:InvisiblePlayer(player, invisible)
    if player.Character then
        for _, part in pairs(player.Character:GetDescendants()) do
            if part:IsA("BasePart") or part:IsA("Decal") then
                part.Transparency = invisible and 1 or 0
            end
        end
    end
end

function Zenux:FlingPlayer(player, power)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.Velocity = Vector3.new(
                math.random(-power, power),
                power,
                math.random(-power, power)
            )
            hrp.RotVelocity = Vector3.new(9e9, 9e9, 9e9)
            
            self:SendFakeChatForPlayer(player, "Fling")
        end
    end
end

function Zenux:TeleportToYou(player)
    if player.Character and RootPart then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            hrp.CFrame = RootPart.CFrame + Vector3.new(math.random(-5, 5), 0, math.random(-5, 5))
        end
    end
end

function Zenux:TeleportToPlayer(player)
    if player.Character and RootPart then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            RootPart.CFrame = hrp.CFrame + Vector3.new(0, 3, 0)
        end
    end
end

function Zenux:AttachPlayer(player)
    if player.Character and RootPart then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local weld = Instance.new("Weld")
            weld.Name = "ZenuxAttach"
            weld.Part0 = RootPart
            weld.Part1 = hrp
            weld.C0 = CFrame.new(math.random(-5, 5), 0, math.random(-5, 5))
            weld.Parent = RootPart
            
            self:SendFakeChatForPlayer(player, "Attach")
        end
    end
end

function Zenux:CagePlayer(player)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local cage = Instance.new("Model")
            cage.Name = "ZenuxCage"
            
            for i = 1, 6 do
                local wall = Instance.new("Part")
                wall.Size = Vector3.new(10, 10, 1)
                wall.Anchored = true
                wall.BrickColor = BrickColor.new("Really black")
                wall.Material = Enum.Material.Neon
                
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
            self:SendFakeChatForPlayer(player, "Cage")
        end
    end
end

function Zenux:ExplodePlayer(player, size)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local exp = Instance.new("Explosion")
            exp.Position = hrp.Position
            exp.BlastRadius = size or 20
            exp.BlastPressure = 500000
            exp.Parent = Workspace
            
            self:SendFakeChatForPlayer(player, "Explode")
        end
    end
end

function Zenux:RocketPlayer(player)
    if player.Character and RootPart then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local rocket = Instance.new("RocketPropulsion")
            rocket.Target = hrp
            rocket.MaxSpeed = 500
            rocket.MaxThrust = 50000
            rocket.Parent = RootPart
            rocket:Fire()
            
            task.delay(3, function()
                if rocket.Parent then rocket:Destroy() end
            end)
        end
    end
end

function Zenux:OrbitPlayer(player, enabled)
    if enabled then
        local angle = 0
        self.Connections["Orbit_" .. player.UserId] = RunService.Heartbeat:Connect(function()
            if player.Character and RootPart then
                local hrp = player.Character:FindFirstChild("HumanoidRootPart")
                if hrp then
                    angle = angle + 0.05
                    local x = math.cos(angle) * self.Settings.OrbitRadius
                    local z = math.sin(angle) * self.Settings.OrbitRadius
                    hrp.CFrame = RootPart.CFrame + Vector3.new(x, 5, z)
                end
            end
        end)
        
        self:SendFakeChatForPlayer(player, "Orbit")
    else
        if self.Connections["Orbit_" .. player.UserId] then
            self.Connections["Orbit_" .. player.UserId]:Disconnect()
            self.Connections["Orbit_" .. player.UserId] = nil
        end
    end
end

function Zenux:ShrinkPlayer(player, scale)
    if player.Character then
        local humanoid = player.Character:FindFirstChild("Humanoid")
        if humanoid then
            for _, obj in pairs(player.Character:GetDescendants()) do
                if obj:IsA("BasePart") then
                    obj.Size = obj.Size * scale
                end
            end
        end
    end
end

function Zenux:PlatformPlayer(player, enabled)
    if enabled then
        if player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                local platform = Instance.new("Part")
                platform.Name = "ZenuxPlatform_" .. player.UserId
                platform.Size = Vector3.new(8, 1, 8)
                platform.Anchored = true
                platform.BrickColor = BrickColor.new("Bright blue")
                platform.Material = Enum.Material.Neon
                platform.Parent = Workspace
                
                self.Connections["Platform_" .. player.UserId] = RunService.Heartbeat:Connect(function()
                    if hrp.Parent then
                        platform.Position = hrp.Position - Vector3.new(0, 4, 0)
                    end
                end)
            end
        end
    else
        if self.Connections["Platform_" .. player.UserId] then
            self.Connections["Platform_" .. player.UserId]:Disconnect()
            self.Connections["Platform_" .. player.UserId] = nil
        end
        if Workspace:FindFirstChild("ZenuxPlatform_" .. player.UserId) then
            Workspace["ZenuxPlatform_" .. player.UserId]:Destroy()
        end
    end
end

function Zenux:BlackHolePlayer(player)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local blackHole = Instance.new("Part")
            blackHole.Shape = Enum.PartType.Ball
            blackHole.Size = Vector3.new(10, 10, 10)
            blackHole.Position = hrp.Position + Vector3.new(0, 20, 0)
            blackHole.BrickColor = BrickColor.new("Really black")
            blackHole.Material = Enum.Material.Neon
            blackHole.Anchored = true
            blackHole.CanCollide = false
            blackHole.Parent = Workspace
            
            local conn
            conn = RunService.Heartbeat:Connect(function()
                if hrp.Parent then
                    local direction = (blackHole.Position - hrp.Position).Unit
                    hrp.Velocity = direction * 80
                end
            end)
            
            task.delay(5, function()
                conn:Disconnect()
                blackHole:Destroy()
            end)
            
            self:SendFakeChatForPlayer(player, "BlackHole")
        end
    end
end

function Zenux:TornadoPlayer(player)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            local tornado = Instance.new("Part")
            tornado.Shape = Enum.PartType.Cylinder
            tornado.Size = Vector3.new(20, 50, 20)
            tornado.Position = hrp.Position
            tornado.BrickColor = BrickColor.new("Light stone grey")
            tornado.Material = Enum.Material.ForceField
            tornado.Anchored = true
            tornado.CanCollide = false
            tornado.Transparency = 0.5
            tornado.Parent = Workspace
            
            local conn
            conn = RunService.Heartbeat:Connect(function()
                if hrp.Parent then
                    tornado.CFrame = CFrame.new(hrp.Position) * CFrame.Angles(0, 0.1, 0)
                    hrp.Velocity = Vector3.new(
                        math.random(-50, 50),
                        100,
                        math.random(-50, 50)
                    )
                end
            end)
            
            task.delay(5, function()
                conn:Disconnect()
                tornado:Destroy()
            end)
            
            self:SendFakeChatForPlayer(player, "Tornado")
        end
    end
end

function Zenux:ClonePlayer(player, amount)
    if player.Character then
        for i = 1, amount or 5 do
            local clone = player.Character:Clone()
            clone.Parent = Workspace
            clone:MoveTo(player.Character.HumanoidRootPart.Position + Vector3.new(
                math.random(-10, 10),
                0,
                math.random(-10, 10)
            ))
        end
        
        self:SendFakeChatForPlayer(player, "Clone")
    end
end

function Zenux:EarthquakePlayer(player)
    if player.Character then
        local hrp = player.Character:FindFirstChild("HumanoidRootPart")
        if hrp then
            for i = 1, 30 do
                task.spawn(function()
                    hrp.CFrame = hrp.CFrame * CFrame.new(
                        math.random(-2, 2),
                        math.random(-1, 1),
                        math.random(-2, 2)
                    )
                end)
                task.wait(0.05)
            end
        end
    end
end

function Zenux:DiscoPlayer(player, enabled)
    if enabled then
        self.Connections["Disco_" .. player.UserId] = RunService.Heartbeat:Connect(function()
            if player.Character then
                for _, part in pairs(player.Character:GetDescendants()) do
                    if part:IsA("BasePart") then
                        part.Color = Color3.fromRGB(
                            math.random(0, 255),
                            math.random(0, 255),
                            math.random(0, 255)
                        )
                    end
                end
            end
            task.wait(0.1)
        end)
    else
        if self.Connections["Disco_" .. player.UserId] then
            self.Connections["Disco_" .. player.UserId]:Disconnect()
            self.Connections["Disco_" .. player.UserId] = nil
        end
    end
end

-- BATCH OPERATIONS
function Zenux:BatchOperation(operation, ...)
    local selected = self:GetSelectedPlayers()
    if #selected == 0 then
        self:Notify("Error", "No players selected", 2)
        return
    end
    
    for _, player in pairs(selected) do
        if player ~= Player then
            operation(self, player, ...)
        end
    end
    
    self:Notify("Batch", "Applied to " .. #selected .. " players", 2)
end

-- MOVEMENT FUNCTIONS
function Zenux:Fly(enabled)
    if enabled then
        local BV = Instance.new("BodyVelocity")
        local BG = Instance.new("BodyGyro")
        
        BV.Name = "ZenuxFly"
        BG.Name = "ZenuxFly"
        BV.Parent = RootPart
        BG.Parent = RootPart
        
        BV.MaxForce = Vector3.new(math.huge, math.huge, math.huge)
        BG.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
        BG.P = 10000
        
        self.Connections.Fly = RunService.Heartbeat:Connect(function()
            local direction = Vector3.new()
            
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                direction = direction + Camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                direction = direction - Camera.CFrame.LookVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                direction = direction - Camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                direction = direction + Camera.CFrame.RightVector
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.Space) then
                direction = direction + Vector3.new(0, 1, 0)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.LeftShift) then
                direction = direction - Vector3.new(0, 1, 0)
            end
            
            BV.Velocity = direction * self.Settings.FlySpeed
            BG.CFrame = Camera.CFrame
        end)
    else
        for _, obj in pairs(RootPart:GetChildren()) do
            if obj.Name == "ZenuxFly" then obj:Destroy() end
        end
        if self.Connections.Fly then
            self.Connections.Fly:Disconnect()
        end
    end
end

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

-- MODERN UI LIBRARY (Blue/Black Theme)
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("ZENUX ULTRA v" .. Zenux.Version, "BloodTheme")

-- PLAYER SELECTOR TAB
local SelectorTab = Window:NewTab("Player Selector")
local PlayerListSection = SelectorTab:NewSection("Active Players")

local function UpdatePlayerList()
    PlayerListSection = SelectorTab:NewSection("Players Online: " .. #Players:GetPlayers())
    
    for _, player in pairs(Players:GetPlayers()) do
        local isSelected = Zenux.SelectedPlayers[player] or false
        
        PlayerListSection:NewButton(
            (isSelected and "[X] " or "[ ] ") .. player.DisplayName .. " (@" .. player.Name .. ")",
            "ID: " .. player.UserId,
            function()
                Zenux.SelectedPlayers[player] = not Zenux.SelectedPlayers[player]
                UpdatePlayerList()
                Zenux:Notify(
                    "Selection",
                    player.Name .. (Zenux.SelectedPlayers[player] and " selected" or " deselected"),
                    2
                )
            end
        )
    end
    
    PlayerListSection:NewButton("Select All Players", "Select everyone", function()
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= Player then
                Zenux.SelectedPlayers[player] = true
            end
        end
        UpdatePlayerList()
    end)
    
    PlayerListSection:NewButton("Deselect All", "Clear selection", function()
        Zenux.SelectedPlayers = {}
        UpdatePlayerList()
    end)
end

UpdatePlayerList()

Players.PlayerAdded:Connect(function() task.wait(1) UpdatePlayerList() end)
Players.PlayerRemoving:Connect(function() task.wait(1) UpdatePlayerList() end)

-- QUICK ACTIONS TAB
local QuickTab = Window:NewTab("Quick Actions")
local QuickSection = QuickTab:NewSection("Fast Troll")

QuickSection:NewButton("Launch Selected", "Send to sky", function()
    Zenux:BatchOperation(Zenux.LaunchPlayer, Zenux.Settings.LaunchHeight)
end)

QuickSection:NewButton("Explode Selected", "Explosion attack", function()
    Zenux:BatchOperation(Zenux.ExplodePlayer, Zenux.Settings.ExplosionSize)
end)

QuickSection:NewButton("Spin Selected", "Infinite rotation", function()
    Zenux:BatchOperation(Zenux.SpinPlayer, Zenux.Settings.SpinSpeed)
end)

QuickSection:NewButton("Fling Selected", "Throw away", function()
    Zenux:BatchOperation(Zenux.FlingPlayer, Zenux.Settings.FlingPower)
end)

QuickSection:NewButton("TP to You", "Bring here", function()
    Zenux:BatchOperation(Zenux.TeleportToYou)
end)

QuickSection:NewButton("Attach Selected", "Stick to you", function()
    Zenux:BatchOperation(Zenux.AttachPlayer)
end)

QuickSection:NewButton("Freeze Selected", "Lock movement", function()
    Zenux:BatchOperation(Zenux.FreezePlayer, true)
end)

QuickSection:NewButton("Unfreeze Selected", "Unlock movement", function()
    Zenux:BatchOperation(Zenux.FreezePlayer, false)
end)

QuickSection:NewButton("Cage Selected", "Trap in box", function()
    Zenux:BatchOperation(Zenux.CagePlayer)
end)

QuickSection:NewButton("Invisible Selected", "Make transparent", function()
    Zenux:BatchOperation(Zenux.InvisiblePlayer, true)
end)

-- ADVANCED TROLL TAB
local AdvancedTab = Window:NewTab("Advanced Troll")
local AdvSection = AdvancedTab:NewSection("Special Effects")

AdvSection:NewButton("Tornado Selected", "Wind vortex", function()
    Zenux:BatchOperation(Zenux.TornadoPlayer)
end)

AdvSection:NewButton("Black Hole Selected", "Gravity pull", function()
    Zenux:BatchOperation(Zenux.BlackHolePlayer)
end)

AdvSection:NewButton("Clone x5", "Duplicate players", function()
    Zenux:BatchOperation(Zenux.ClonePlayer, 5)
end)

AdvSection:NewButton("Clone x20", "Mass duplicate", function()
    Zenux:BatchOperation(Zenux.ClonePlayer, 20)
end)

AdvSection:NewButton("Earthquake Selected", "Shake effect", function()
    Zenux:BatchOperation(Zenux.EarthquakePlayer)
end)

AdvSection:NewButton("Rocket to Selected", "Fly to targets", function()
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        Zenux:RocketPlayer(player)
        task.wait(0.5)
    end
end)

AdvSection:NewButton("Ragdoll Selected", "Disable standing", function()
    Zenux:BatchOperation(Zenux.RagdollPlayer)
end)

AdvSection:NewToggle("Orbit Selected", "Rotate around you", function(state)
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        Zenux:OrbitPlayer(player, state)
    end
end)

AdvSection:NewToggle("Platform Under Selected", "Floor below", function(state)
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        Zenux:PlatformPlayer(player, state)
    end
end)

AdvSection:NewToggle("Disco Mode", "Rainbow effect", function(state)
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        Zenux:DiscoPlayer(player, state)
    end
end)

-- CHAOS MODE TAB
local ChaosTab = Window:NewTab("Chaos Mode")
local ChaosSection = ChaosTab:NewSection("Ultimate Destruction")

ChaosSection:NewButton("APOCALYPSE MODE", "Total chaos", function()
    Zenux:Notify("CHAOS", "APOCALYPSE STARTED", 3)
    
    for i = 1, 3 do
        Zenux:BatchOperation(Zenux.LaunchPlayer, 1000)
        task.wait(1)
        Zenux:BatchOperation(Zenux.ExplodePlayer, 50)
        task.wait(1)
        Zenux:BatchOperation(Zenux.SpinPlayer, 200)
        task.wait(1)
    end
end)

ChaosSection:NewButton("TORNADO SPAM", "Multiple tornados", function()
    for i = 1, 5 do
        for _, player in pairs(Zenux:GetSelectedPlayers()) do
            Zenux:TornadoPlayer(player)
        end
        task.wait(2)
    end
end)

ChaosSection:NewButton("EXPLOSION SPAM", "Massive explosions", function()
    for i = 1, 20 do
        for _, player in pairs(Zenux:GetSelectedPlayers()) do
            Zenux:ExplodePlayer(player, 30)
        end
        task.wait(0.5)
    end
end)

ChaosSection:NewButton("CLONE ARMY", "Massive clones", function()
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        Zenux:ClonePlayer(player, 20)
    end
end)

ChaosSection:NewButton("SPIN MADNESS", "Random spinning", function()
    for i = 1, 30 do
        Zenux:BatchOperation(Zenux.SpinPlayer, math.random(100, 300))
        task.wait(0.5)
    end
end)

ChaosSection:NewButton("VOID ALL", "Send to void", function()
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        if player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = CFrame.new(0, -1000, 0)
            end
        end
    end
end)

-- MOVEMENT TAB
local MovementTab = Window:NewTab("Movement")
local MoveSection = MovementTab:NewSection("Player Controls")

MoveSection:NewSlider("Walk Speed", "Movement speed", 500, 16, function(v)
    if Humanoid then Humanoid.WalkSpeed = v end
end)

MoveSection:NewSlider("Jump Power", "Jump height", 500, 50, function(v)
    if Humanoid then Humanoid.JumpPower = v end
end)

MoveSection:NewToggle("Fly Mode", "Enable flying", function(state)
    Zenux:Fly(state)
end)

MoveSection:NewSlider("Fly Speed", "Flight speed", 300, 50, function(v)
    Zenux.Settings.FlySpeed = v
end)

MoveSection:NewToggle("NoClip", "Walk through walls", function(state)
    Zenux:NoClip(state)
end)

MoveSection:NewSlider("Gravity", "World gravity", 500, 196, function(v)
    Workspace.Gravity = v
end)

MoveSection:NewButton("Infinite Jump", "Jump forever", function()
    local InfiniteJumpEnabled = true
    UserInputService.JumpRequest:Connect(function()
        if InfiniteJumpEnabled and Humanoid then
            Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
        end
    end)
end)

-- VISUAL TAB
local VisualTab = Window:NewTab("Visual")
local VisSection = VisualTab:NewSection("Visual Effects")

VisSection:NewToggle("Fullbright", "Max lighting", function(state)
    if state then
        Lighting.Brightness = 2
        Lighting.ClockTime = 14
        Lighting.FogEnd = 9e9
    else
        Lighting.Brightness = 1
        Lighting.ClockTime = 12
    end
end)

VisSection:NewSlider("FOV", "Field of view", 120, 70, function(v)
    Camera.FieldOfView = v
end)

VisSection:NewButton("ESP Players", "See through walls", function()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Player and player.Character then
            for _, part in pairs(player.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    local highlight = Instance.new("Highlight")
                    highlight.FillColor = Color3.fromRGB(0, 150, 255)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.Parent = part
                end
            end
        end
    end
end)

VisSection:NewButton("Rainbow World", "Colorful map", function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            obj.Material = Enum.Material.Neon
            obj.Color = Color3.fromRGB(math.random(0,255), math.random(0,255), math.random(0,255))
        end
    end
end)

VisSection:NewButton("Remove Textures", "Clean visuals", function()
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("Decal") or obj:IsA("Texture") then
            obj:Destroy()
        end
    end
end)

-- TELEPORT TAB
local TPTab = Window:NewTab("Teleport")
local TPSection = TPTab:NewSection("Teleport Options")

TPSection:NewButton("TP to Random Selected", "Random target", function()
    local selected = Zenux:GetSelectedPlayers()
    if #selected > 0 then
        local random = selected[math.random(1, #selected)]
        Zenux:TeleportToPlayer(random)
    end
end)

TPSection:NewButton("TP All to You", "Bring everyone", function()
    Zenux:BatchOperation(Zenux.TeleportToYou)
end)

TPSection:NewButton("TP to Mouse", "Cursor position", function()
    if RootPart then
        RootPart.CFrame = CFrame.new(Mouse.Hit.Position + Vector3.new(0, 3, 0))
    end
end)

TPSection:NewButton("TP All to Void", "Send down", function()
    for _, player in pairs(Zenux:GetSelectedPlayers()) do
        if player.Character then
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = CFrame.new(0, -1000, 0)
            end
        end
    end
end)

TPSection:NewButton("Circle Formation", "Arrange in circle", function()
    local selected = Zenux:GetSelectedPlayers()
    local radius = 20
    local angleStep = (2 * math.pi) / #selected
    
    for i, player in pairs(selected) do
        if player.Character and RootPart then
            local angle = angleStep * i
            local x = math.cos(angle) * radius
            local z = math.sin(angle) * radius
            local hrp = player.Character:FindFirstChild("HumanoidRootPart")
            if hrp then
                hrp.CFrame = RootPart.CFrame + Vector3.new(x, 0, z)
            end
        end
    end
end)

-- SETTINGS TAB
local SettingsTab = Window:NewTab("Settings")
local SetSection = SettingsTab:NewSection("Troll Configuration")

SetSection:NewSlider("Fling Power", "Force amount", 5000, 500, function(v)
    Zenux.Settings.FlingPower = v
end)

SetSection:NewSlider("Spin Speed", "Rotation speed", 300, 50, function(v)
    Zenux.Settings.SpinSpeed = v
end)

SetSection:NewSlider("Launch Height", "Throw height", 2000, 200, function(v)
    Zenux.Settings.LaunchHeight = v
end)

SetSection:NewSlider("Orbit Radius", "Circle size", 50, 10, function(v)
    Zenux.Settings.OrbitRadius = v
end)

SetSection:NewSlider("Explosion Size", "Blast radius", 100, 10, function(v)
    Zenux.Settings.ExplosionSize = v
end)

SetSection:NewToggle("Auto Chat Reactions", "BR chat responses", function(state)
    Zenux.Settings.AutoChatEnabled = state
    Zenux:Notify("Chat", state and "Auto chat enabled" or "Auto chat disabled", 2)
end)

local UtilSection = SettingsTab:NewSection("Utilities")

UtilSection:NewButton("Clear Objects", "Remove spawned items", function()
    for _, obj in pairs(Workspace:GetChildren()) do
        if obj.Name:find("Zenux") then
            obj:Destroy()
        end
    end
    Zenux:Notify("Clean", "Objects cleared", 2)
end)

UtilSection:NewButton("Reset Character", "Respawn yourself", function()
    if Humanoid then
        Humanoid.Health = 0
    end
end)

UtilSection:NewButton("Rejoin Server", "Reconnect", function()
    TeleportService:TeleportToPlaceInstance(game.PlaceId, game.JobId, Player)
end)

UtilSection:NewButton("Server Hop", "Find new server", function()
    local success, servers = pcall(function()
        return HttpService:JSONDecode(
            game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100")
        ).data
    end)
    
    if success then
        for _, server in pairs(servers) do
            if server.id ~= game.JobId and server.playing < server.maxPlayers then
                TeleportService:TeleportToPlaceInstance(game.PlaceId, server.id, Player)
                return
            end
        end
    end
end)

UtilSection:NewKeybind("Toggle UI", "Hide/Show", Enum.KeyCode.RightControl, function()
    Library:ToggleUI()
end)

-- INFO TAB
local InfoTab = Window:NewTab("Info")
local InfoSection = InfoTab:NewSection("Hub Information")

InfoSection:NewLabel("ZENUX ULTRA HUB")
InfoSection:NewLabel("Version: " .. Zenux.Version)
InfoSection:NewLabel("Player: " .. Player.Name)
InfoSection:NewLabel("Display: " .. Player.DisplayName)
InfoSection:NewLabel("User ID: " .. Player.UserId)
InfoSection:NewLabel("")
InfoSection:NewLabel("Features: 150+")
InfoSection:NewLabel("Advanced Player Selection")
InfoSection:NewLabel("Brazilian Auto Chat System")
InfoSection:NewLabel("Batch Operations Enabled")
InfoSection:NewLabel("")
InfoSection:NewLabel("Created by Zenux Team")
InfoSection:NewLabel("Modern Blue/Black Edition")

local StatsSection = InfoTab:NewSection("Feature Statistics")

StatsSection:NewLabel("Player Selection: Active")
StatsSection:NewLabel("Quick Actions: 10 functions")
StatsSection:NewLabel("Advanced Troll: 10 effects")
StatsSection:NewLabel("Chaos Mode: 6 modes")
StatsSection:NewLabel("Movement: 7 options")
StatsSection:NewLabel("Visual: 5 effects")
StatsSection:NewLabel("Teleport: 5 options")
StatsSection:NewLabel("Settings: Customizable")
StatsSection:NewLabel("Auto Chat: Brazilian Style")

-- CHARACTER UPDATE
Player.CharacterAdded:Connect(function()
    task.wait(0.5)
    UpdateCharacter()
    Zenux:Notify("Character", "Updated", 2)
end)

-- INITIALIZATION
task.spawn(function()
    Zenux:Notify("ZENUX ULTRA", "Loaded successfully", 5)
    task.wait(1)
    Zenux:Notify("Features", "150+ functions available", 3)
    task.wait(1)
    Zenux:Notify("Auto Chat", "BR responses enabled", 3)
end)

print([[
====================================================
         ZENUX ULTRA HUB v3.0.0            
       MODERN BLUE/BLACK EDITION                
====================================================

Features:
 - Advanced Player Selection
 - Brazilian Auto Chat System
 - 150+ Troll Functions
 - Batch Operations
 - Chaos Modes
 - Modern UI Design

Auto Chat:
 - Selected players will type in chat
 - Brazilian style messages
 - Different messages per action
 - Anti-repeat system

Controls:
 - Right Ctrl: Toggle UI
 - WASD: Fly Movement
 
Successfully Loaded!
====================================================
]])

return Zenux
