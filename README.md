getgenv().Dexul = {
    Options = {
        ['Key'] = "Your Key Here", -- // Your Luarmor Key.
        ['Intro'] = true, -- // Shows a Intro once injected.
        ['Type'] = "Notification" -- // Notification, Image.
    },
    Silent_Aim = {
        ['Keybind'] = "p", -- // Toggle Key For Silent Aim
        ['Enabled'] = false, -- // Self Explanitory
        ['Mode'] = "Target", -- // Target Or Normal
        ['Aiming_Type'] = "Point", -- // Point, Part, Default, WIP.
        ['ClosestPart'] = true, -- // Gets Closest Part On The Player
        ['Predict'] = true,
        ['Prediction'] =  0.1398, -- // Prediction Of Silent
        ['HitChance'] = 200, -- // Hit Changc To Hit More/Less Shots
        ['Part'] = "Head", -- // HitPart of where the silent shoots.
    },
    Aim_Assist = {
        ['Key'] = "Q", -- // Toggle key of aim assist
        ['Enabled'] = true, -- // Self-explanatory
        ['Prediction'] =  0.058, -- // Prediction on the aim assist
        ['Smoothness'] = 1, -- // Smoothness of the aim assist
        ['AimPart'] = "Head",
        ['DisableOutSideFov'] = true,
        ['Nearest_Part'] = false,
    },    
    FOV = {
        ['Visible'] = false, -- // // FOV visible or not
        ['Radius'] = 12, -- // FOV radius
        ['Color'] = Color3.fromRGB(0, 162, 255), -- // Color of FOV
        ['Filled'] = false,
        ['Transparency'] = 0.25,
        ['Thickness'] = 1, -- // Thickness of FOV
        ['NumSides'] = 32
    },
    Shake = {
        ['Shake_Enabled'] = true,
        ['X'] = 06,
        ['Y'] = 06,
        ['Z'] = 05
    },
    Easing_Styles = {
        ['Easing1'] = Enum.EasingStyle.Elastic, -- // Style
        ['Easing2'] = Enum.EasingStyle.Sine -- // Direction
    },
    GunFOV = { -- // Changes FOV On Specific Gun.
        ['Enabled'] = false,
        ['DoubleBarrel'] = 14.4,
        ['Revolver'] = 12.2,
        ['TacticalShotgun'] = 15.1,
        ['Shotgun'] = 15.1,
        ['Silencer'] = 12.2,
        ['P90'] = 11.4
    },
	AutoPrediction = {
        ['Enabled'] = false,
        ['RefreshRate'] = 0.1,
        ['Ping_10'] = 0.10,
        ['Ping_20'] = 0.11,
        ['Ping_30'] = 0.12,
        ['Ping_40'] = 0.123,
        ['Ping_50'] = 0.125,
        ['Ping_60'] = 0.127,
        ['Ping_70'] = 0.133,
        ['Ping_80'] = 0.134,
        ['Ping_90'] = 0.1365,
        ['Ping_100'] = 0.1374,
        ['Ping_110'] = 0.12,
        ['Ping_120'] = 0.12,
        ['Ping_130'] = 0.12,
        ['Ping_140'] = 0.13,
        ['Ping_150'] = 0.1405,
    },    
    FPS = {
        ['FPSUnlocker'] = true, -- // Unlocks your fps.
        ['FPSBoost'] = true, -- // Turns On Low GFX
        ['Limit'] = 300 -- // Limit Of The FPS You Want
    },    
    Checks = { -- // Da Hood Checks, ETC.
        ['Death'] = true, -- // Checks if you are dead.
        ['Knocked'] = true, -- // Checks If The Target Is Knocked
        ['Wall'] = true, -- // Checks If Target Is Not In Sight
        ['Crew'] = false, -- // Won't Lock Onto Them If They Are In Your Crew
        ['Friend'] = true, -- // Checks If They Are On Your Friends list

    },
    Macro = {
        ['Enabled'] = false, -- // Self Explanitory
        ['Keybind'] = "q", -- // Keybind Of Macro
        ['Type'] = "Normal", -- // Normal Or Mouse, WIP
    }
}


    if getgenv().CheckIfScriptLoaded == true then
game.StarterGui:SetCore("SendNotification",  {
 Title = "Dexul";
 Text = "Loaded New Config.";
 Icon = "";
 Duration = 5;
 })
        
        return
    end
    
getgenv().CheckIfScriptLoaded = true

local function getnamecall()
    if game.PlaceId == 2788229376 then
        return "UpdateMousePos"
    elseif game.PlaceId == 5602055394 or game.PlaceId == 7951883376 then
        return "MousePos"
    elseif game.PlaceId == 9825515356 then
        return "GetMousePos"
    end
end

local namecalltype = getnamecall()

function MainEventLocate()
    for _,v in pairs(game:GetService("ReplicatedStorage"):GetDescendants()) do
        if v.Name == "MainEvent" then
            return v
        end
    end
end

local mainevent = MainEventLocate()
local character = game.Players.LocalPlayer.Character
local humanoid = character.Humanoid
-- Camlock Info

getgenv().partt = "Head"

local Prey = nil
local Plr = nil

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

local Dexul = getgenv().Dexul

local Circle = Drawing.new("Circle")
Circle.Color = getgenv().Dexul.FOV.Color
Circle.Thickness = getgenv().Dexul.FOV.Thickness
Circle.Filled = getgenv().Dexul.FOV.Filled
Circle.Transparency = getgenv().Dexul.FOV.Transparency
Circle.NumSides = getgenv().Dexul.FOV.NumSides
local UpdateFOV = function()
    if (not Circle) then
        return Circle
    end
    Circle.Visible = getgenv().Dexul.FOV.Visible
    Circle.Radius = getgenv().Dexul.FOV.Radius * 3
    Circle.Position = Vector2.new(Mouse.X, Mouse.Y + (game:GetService("GuiService"):GetGuiInset().Y))
    return Circle
end

if getgenv().Dexul.FPS.FPSUnlocker == true then
    if setfpscap then
        setfpscap(getgenv().Dexul.FPS.Limit)
    end
end

RunService.Heartbeat:Connect(UpdateFOV)

-- Closest PLR
ClosestPlrFromMouse = function()
    local Target, Closest = nil, 1 / 0

    for _, v in pairs(Players:GetPlayers()) do
        if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
            local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
            local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude

            if (Circle.Radius > Distance and Distance < Closest and OnScreen) then
                Closest = Distance
                Target = v
            end
        end
    end
    return Target
end

local WTS = function(Object)
    local ObjectVector = Camera:WorldToScreenPoint(Object.Position)
    return Vector2.new(ObjectVector.X, ObjectVector.Y)
end

local IsOnScreen = function(Object)
    local IsOnScreen = Camera:WorldToScreenPoint(Object.Position)
    return IsOnScreen
end

local FilterObjs = function(Object)
    if string.find(Object.Name, "Gun") then
        return
    end
    if table.find({"Part", "MeshPart", "BasePart"}, Object.ClassName) then
        return true
    end
end

if getgenv().Dexul.GunFOV.Enabled == true then
    game:GetService("Players").LocalPlayer.Character.ChildAdded:Connect(
        function(gun)
            local name = gun.Name
            if gun.Name == "[Revolver]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.Revolver
            end
            if gun.Name == "[Double-Barrel SG]" or "[Double-Barrel]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.DoubleBarrel
            end
            if gun.Name == "[Shotgun]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.Shotgun
            end
            if gun.Name == "[TacticalShotgun]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.TacticalShotgun
            end
            if gun.Name == "[Silencer]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.Silencer
            end
            if gun.Name == "[P90]" then
                Dexul.FOV.Radius = getgenv().Dexul.GunFOV.P90
            end
        end
    )
end

local GetClosestBodyPart = function(character)
    local ClosestDistance = 1 / 0
    local BodyPart = nil
    if getgenv().Dexul.Aim_Assist.Nearest_Part then
        if (character and character:GetChildren()) then
            for _, x in next, character:GetChildren() do
                if FilterObjs(x) and IsOnScreen(x) then
                    local Distance = (WTS(x) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                    if (Circle.Radius > Distance and Distance < ClosestDistance) then
                        ClosestDistance = Distance
                        BodyPart = x
                    end
                end
            end
        end
        return BodyPart
    end
end

local Prey

task.spawn(
    function()
        while task.wait() do
            if Prey and getgenv().Dexul.Silent_Aim.Enabled and getgenv().Dexul.Silent_Aim.ClosestPart then
                getgenv().Dexul.Silent_Aim.Part = tostring(GetClosestBodyPart(Prey.Character))
            end
        end
    end
)

local CC = game.Workspace.CurrentCamera
local Mouse = game.Players.LocalPlayer:GetMouse()
local Plr
function GetClosest()
    local closestPlayer
    local shortestDistance = math.huge
    for i, v in pairs(game.Players:GetPlayers()) do
        pcall(
            function()
                if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") then
                    local pos = CC:WorldToViewportPoint(v.Character.PrimaryPart.Position)
                    local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude
                    if (Vector2.new(pos.X, pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).magnitude < shortestDistance then
                        closestPlayer = v
                        shortestDistance = magnitude
                    end
                end
            end
        )
    end
    return closestPlayer
end

Mouse.KeyDown:Connect(
    function(Key)
        local Keybind = getgenv().Dexul.Aim_Assist.Key:lower()
        if (Key == Keybind) then
            if getgenv().Dexul.Aim_Assist.Enabled == true then
                IsTargetting = not IsTargetting
                if IsTargetting then
                    Plr = GetClosest()
                else
                    if Plr ~= nil then
                        Plr = nil
                    end
                end
            end
        end
    end
)
local function uwuCheckAnti(Target) -- // Anti-aim detection
    if
        (Target.Character.HumanoidRootPart.Velocity.Y < -5 and
            Target.Character.Humanoid:GetState() ~= Enum.HumanoidStateType.Freefall) or
            Target.Character.HumanoidRootPart.Velocity.Y < -50
     then
        return true
    elseif
        Target and
            (Target.Character.HumanoidRootPart.Velocity.X > 35 or Target.Character.HumanoidRootPart.Velocity.X < -35)
     then
        return true
    elseif Target and Target.Character.HumanoidRootPart.Velocity.Y > 60 then
        return true
    elseif
        Target and
            (Target.Character.HumanoidRootPart.Velocity.Z > 35 or Target.Character.HumanoidRootPart.Velocity.Z < -35)
     then
        return true
    else
        return false
    end
end
local function IsOnScreen(Object)
    local IsOnScreen = game.Workspace.CurrentCamera:WorldToScreenPoint(Object.Position)
    return IsOnScreen
end

local function Filter(Object)
    if string.find(Object.Name, "Gun") then
        return
    end
    if Object:IsA("Part") or Object:IsA("MeshPart") then
        return true
    end
end

local function WTSPos(Position)
    local ObjectVector = game.Workspace.CurrentCamera:WorldToScreenPoint(Position)
    return Vector2.new(ObjectVector.X, ObjectVector.Y)
end

local function WTS(Object)
    local ObjectVector = game.Workspace.CurrentCamera:WorldToScreenPoint(Object.Position)
    return Vector2.new(ObjectVector.X, ObjectVector.Y)
end

function GetNearestPartToCursorOnCharacter(character)
    local ClosestDistance = math.huge
    local BodyPart = nil

    if (character and character:GetChildren()) then
        for k, x in next, character:GetChildren() do
            if Filter(x) and IsOnScreen(x) then
                local Distance = (WTS(x) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude

                if Distance < ClosestDistance then
                    ClosestDistance = Distance
                    BodyPart = x
                end
            end
        end
    end

    return BodyPart
end

Mouse.KeyDown:Connect(
    function(Key)
        local Keybind = getgenv().Dexul.Silent_Aim.Keybind:lower()
        if (Key == Keybind) then
            if getgenv().Dexul.Silent_Aim.Enabled == true then
                getgenv().Dexul.Silent_Aim.Enabled = false
                if getgenv().Dexul.Options.Notifications == true then
                    warn("Silent-Aim Disabled")
                end
            else
                getgenv().Dexul.Silent_Aim.Enabled = true
                if getgenv().Dexul.Options.Notifications == true then
                    warn("Silent-Aim Enabled")
                end
            end
        end
    end
)




        if getgenv().Dexul.Checks.Death == true and Plr and Plr.Character:FindFirstChild("Humanoid") then
            if Plr.Character.Humanoid.health < 2 then
                Plr = nil
                IsTargetting = false
            end
        end
        if getgenv().Dexul.Checks.Death == true and Plr and Plr.Character:FindFirstChild("Humanoid") then
            if Client.Character.Humanoid.health < 2 then
                Plr = nil
                IsTargetting = false
            end
        end
        if getgenv().Dexul.Checks.Knocked == true and Prey and Prey.Character then
            local KOd = Prey.Character:WaitForChild("BodyEffects")["K.O"].Value
            local Grabbed = Prey.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
            if KOd or Grabbed then
                Prey = nil
            end
        end
        if getgenv().Dexul.Checks.Knocked == true and Plr and Plr.Character then
            local KOd = Plr.Character:WaitForChild("BodyEffects")["K.O"].Value
            local Grabbed = Plr.Character:FindFirstChild("GRABBING_CONSTRAINT") ~= nil
            if KOd or Grabbed then
                Plr = nil
                IsTargetting = false
            end
        end


local grmt = getrawmetatable(game)
local backupindex = grmt.__index
setreadonly(grmt, false)

local grmt = getrawmetatable(game)
local backupindex = grmt.__index
setreadonly(grmt, false)

grmt.__index =
    newcclosure(
    function(self, v)
        if (getgenv().Dexul.Silent_Aim.Enabled and Mouse and tostring(v) == "Hit") then
            if getgenv().Dexul.Silent_Aim.Mode == "Target" then
                Prey = Plr
            elseif getgenv().Dexul.Silent_Aim.Mode == "Normal" then
                Prey = ClosestPlrFromMouse()
            end

            if Prey then
                local endpoint =
                    mainevent:FireServer(namecalltype, game.Players[tostring(Prey)].Character[getgenv().Dexul.Silent_Aim.Part].Position + (game.Players[tostring(Prey)].Character[getgenv().Dexul.Silent_Aim.Part].Velocity * getgenv().Dexul.Silent_Aim.Prediction))
                return (tostring(v) == "Hit" and endpoint)
            end
        end
        return backupindex(self, v)
    end
)


task.spawn(
    function()
        while task.wait() do
            if getgenv().Dexul.Aim_Assist.Enabled and Plr ~= nil and (Plr.Character) then
                if getgenv().Dexul.Aim_Assist.Nearest_Part == true then
                    getgenv().partt = tostring(GetNearestPartToCursorOnCharacter(Plr.Character))
                end
            end
        end
    end
)
local Wall = function(destination, ignore)
    local Origin = Camera.CFrame.p
    local CheckRay = Ray.new(Origin, destination - Origin)
    local Hit = game.workspace:FindPartOnRayWithIgnoreList(CheckRay, ignore)
    return Hit == nil
end
local ClosestPlrFromMouse = function()
    local Target, Closest = nil, 1 / 0

    for _, v in pairs(Players:GetPlayers()) do
        if getgenv().Dexul.Checks.Wall then
            if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
                local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude

                if
                    (Circle.Radius * 1.27 > Distance and Distance < Closest and OnScreen) and
                        Wall(v.Character.HumanoidRootPart.Position, {Client, v.Character})
                 then
                    Closest = Distance
                    Plr = v
                end
            end
        else
            if (v.Character and v ~= Client and v.Character:FindFirstChild("HumanoidRootPart")) then
                local Position, OnScreen = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                local Distance = (Vector2.new(Position.X, Position.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude

                if (Circle.Radius * 1.27 > Distance and Distance < Closest and OnScreen) then
                    Closest = Distance
                    Plr = v
                end
            end
        end
    end
    return Target
end
game.RunService.Heartbeat:Connect(
    function()
        if getgenv().Dexul.Shake.Shake_Enabled then
            local Main =
                CFrame.new(
                Camera.CFrame.p,
                Plr.Character[getgenv().partt].Position +
                    Plr.Character[getgenv().partt].Velocity * getgenv().Dexul.Aim_Assist.Prediction +
                    Vector3.new(
                        math.random(-getgenv().Dexul.Shake.X, getgenv().Dexul.Shake.X),
                        math.random(-getgenv().Dexul.Shake.Y, getgenv().Dexul.Shake.Y),
                        math.random(-getgenv().Dexul.Shake.Z, getgenv().Dexul.Shake.Z)
                    ) *
                        0.1
            )
            Camera.CFrame =
                Camera.CFrame:Lerp(
                Main,
                getgenv().Dexul.Aim_Assist.Smoothness,
                getgenv().Dexul.Easing_Styles.Easing1,
                Enum.EasingDirection.InOut,
                getgenv().Dexul.Easing_Styles.Easing2
            )
        else
            local Main =
                CFrame.new(
                Camera.CFrame.p,
                Plr.Character[getgenv().partt].Position +
                    Plr.Character[getgenv().partt].Velocity * getgenv().Dexul.Aim_Assist.Prediction
            )
            Camera.CFrame =
                Camera.CFrame:Lerp(
                Main,
                getgenv().Dexul.Aim_Assist.Smoothness,
                getgenv().Dexul.Easing_Styles.Easing1,
                Enum.EasingDirection.InOut,
                getgenv().Dexul.Easing_Styles.Easing2
            )
        end
    end
)

if getgenv().Dexul.Checks.Crew then
    while true do
        local newPlayer = game.Players.PlayerAdded:wait()
        if player:IsInGroup(newPlayer.Group) then
            table.insert(Ignored.Players, newPlayer)
        end
    end
end
if getgenv().SDexulplash.Checks.Friend then
    game.Players.PlayerAdded:Connect(
        function(SilentAimTarget)
            if Client:IsFriendsWith(SilentAimTarget) then
                local newPlayer = game.Players.PlayerAdded:wait()
                table.insert(Ignored.Players, newPlayer)
            end
        end
    )
end

local Player = game:GetService("Players").LocalPlayer
local Mouse = Player:GetMouse()
local SpeedGlitch = false
Mouse.KeyDown:Connect(
    function(Key)
        if getgenv().Dexul.Macro.Enabled == true and Key == getgenv().Dexul.Macro.Keybind then
            SpeedGlitch = not SpeedGlitch
            if SpeedGlitch == true then
                repeat
                    game:GetService("RunService").Heartbeat:wait()
                    keypress(0x49)
                    game:GetService("RunService").Heartbeat:wait()

                    keypress(0x4F)
                    game:GetService("RunService").Heartbeat:wait()

                    keyrelease(0x49)
                    game:GetService("RunService").Heartbeat:wait()

                    keyrelease(0x4F)
                    game:GetService("RunService").Heartbeat:wait()
                until SpeedGlitch == false
            end
        end
    end
)

if getgenv().Dexul.Aim_Assist.DisableOutSideFov == true and AimlockTarget and AimlockTarget.Character then
    if
        Circle.Radius <
            (Vec2(
                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).X,
                Camera:WorldToScreenPoint(AimlockTarget.Character.HumanoidRootPart.Position).Y
            ) - Vec2(Mouse.X, Mouse.Y)).Magnitude
     then
        Plr = nil
    end
end

function PredictionictTargets(Target, Value)
    return Plr.Character[getgenv().Dexul.Silent_Aim.Part].CFrame + (Plr.Character.Humanoid.MoveDirection * Value * 16)
end

	while getgenv().Dexul.AutoPrediction.Enabled == true do
    local Ping = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
    local pingNumber = string.split(Ping, " ")[1] 
        if pingNumber < 10 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_10
        elseif pingNumber < 20 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_20
        elseif pingNumber < 30 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_30
        elseif pingNumber < 40 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_40
        elseif pingNumber < 50 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_50
        elseif pingNumber < 60 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_60
        elseif pingNumber < 70 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_70
        elseif pingNumber < 80 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_80
        elseif pingNumber < 90 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_90
        elseif pingNumber < 100 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_100
        elseif pingNumber < 110 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_110
        elseif pingNumber < 120 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_120
        elseif pingNumber < 130 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_130 
        elseif pingNumber < 140 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_140 
        elseif pingNumber < 150 then
        getgenv().Dexul.Silent_Aim.Prediction = getgenv().Dexul.AutoPrediction.Ping_150 
    end
    wait(getgenv().Dexul.AutoPrediction.RefreshRate)
end

if getgenv().Dexul.Options.Intro == true then

local intro = true

if intro then


local Image = Drawing.new("Image")
local Screen = workspace.CurrentCamera.ViewportSize

Image.Data = game:HttpGet("https://i.imgur.com/ZCy6Pcd.png")
Image.Visible = true
Image.Transparency = 1
Image.Position = Vector2.new(100,100)
Image.Size = Vector2.new(Screen.X,Screen.Y)

local Sound = Instance.new("Sound")

Sound.SoundId = "http://www.roblox.com/asset/?id=1091083826"
Sound.Volume = 5
Sound.Parent = game.Workspace

Sound:play()

task.wait(1)

for i = 1, 100 do
    Image.Transparency = 1 - (i / 40)
    task.wait(0.01)
end

Image:Remove()

end
end
