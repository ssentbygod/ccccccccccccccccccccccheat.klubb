-- oh my god kill why do you tap so much
-- ratz v9999 dot lock NiggersK
-- MousePosUpdate For Hood Customs
local settings = {
    uwu = {
        enabled = true,
        key = "q",
        dot = true,
        airshot = true,
        notif = true,
        autopred = true,
        fov = math.huge,
        resolver = false,
        airshot_hitpart = "RightFoot",
        falling_hitpart = "RightFoot",
        regular_hitpart = "HumanoidRootPart"
    }
}

local selectedPart = settings.uwu.regular_hitpart
local prediction = true
local predictionValue = 0.121

local anchorCount = 0
local maxAnchor = 50

local cc = game:GetService("Workspace").CurrentCamera
local plr
local enabled = false
local accomidationfactor = 0.121
local mouse = game.Players.LocalPlayer:GetMouse()
local placemarker = Instance.new("Part", game.Workspace)

function makemarker(parent, adornee, color, size, size2)
    local e = Instance.new("BillboardGui", parent)
    e.Name = "PP"
    e.Adornee = adornee
    e.Size = UDim2.new(size, size2, size, size2)
    e.AlwaysOnTop = settings.uwu.dot
    local a = Instance.new("Frame", e)
    if settings.uwu.dot == true then
        a.Size = UDim2.new(1, 0, 1, 0)
    else
        a.Size = UDim2.new(0, 0, 0, 0)
    end
    if settings.uwu.dot == true then
        a.Transparency = 0
        a.BackgroundTransparency = 0
    else
        a.Transparency = 1
        a.BackgroundTransparency = 1
    end
    a.BackgroundColor3 = color
    local g = Instance.new("UICorner", a)
    if settings.uwu.dot == false then
        g.CornerRadius = UDim.new(0, 0)
    else
        g.CornerRadius = UDim.new(1, 1)
    end
    return e
end

local data = game.Players:GetPlayers()
function noob(player)
    local character
    repeat wait() until player.Character
    local handler = makemarker(guimain, player.Character:WaitForChild(selectedPart), Color3.fromRGB(107, 184, 255), 0.3, 3)
    handler.Name = player.Name
    player.CharacterAdded:connect(function(Char) handler.Adornee = Char:WaitForChild(selectedPart) end)

    spawn(function()
        while wait() do
            if player.Character then
            end
        end
    end)
end

for i = 1, #data do
    if data[i] ~= game.Players.LocalPlayer then
        noob(data[i])
    end
end

game.Players.PlayerAdded:connect(function(Player)
    noob(Player)
end)

spawn(function()
    placemarker.Anchored = true
    placemarker.CanCollide = false
    if settings.uwu.dot == true then
        placemarker.Size = Vector3.new(8, 8, 8)
    else
        placemarker.Size = Vector3.new(0, 0, 0)
    end
    placemarker.Transparency = 0.75
    if settings.uwu.dot then
        makemarker(placemarker, placemarker, Color3.fromRGB(0, 238, 255), 0.40, 0)
    end
end)

game.Players.LocalPlayer:GetMouse().KeyDown:Connect(function(k)
    if k == settings.uwu.key and settings.uwu.enabled then
        if enabled == true then
            enabled = false
            if settings.uwu.notif == true then
                plr = getClosestPlayerToCursor()
                game.StarterGui:SetCore("SendNotification", {
                    Title = "unhittable.cc temp";
                    Text = "unlocked",
                    Duration = 5
                })
            end
        else
            plr = getClosestPlayerToCursor()
            enabled = true
            if settings.uwu.notif == true then
                game.StarterGui:SetCore("SendNotification", {
                    Title = "unhittable.cc temp";
                    Text = "target: "..tostring(plr.Character.Humanoid.DisplayName),
                    Duration = 5
                })
            end
        end
    end
end)

function getClosestPlayerToCursor()
    local closestPlayer
    local shortestDistance = settings.uwu.fov

    for i, v in pairs(game.Players:GetPlayers()) do
        if v ~= game.Players.LocalPlayer and v.Character and v.Character:FindFirstChild("Humanoid") and v.Character.Humanoid.Health ~= 0 and v.Character:FindFirstChild("HumanoidRootPart") then
            local pos = cc:WorldToViewportPoint(v.Character.PrimaryPart.Position)
            local magnitude = (Vector2.new(pos.X, pos.Y) - Vector2.new(mouse.X, mouse.Y)).magnitude
            if magnitude < shortestDistance then
                closestPlayer = v
                shortestDistance = magnitude
            end
        end
    end
    return closestPlayer
end

local pingvalue = nil
local split = nil
local ping = nil

game:GetService("RunService").Stepped:connect(function()
    if enabled and plr.Character ~= nil and plr.Character:FindFirstChild("HumanoidRootPart") then
        placemarker.CFrame = CFrame.new(plr.Character.HumanoidRootPart.Position+(plr.Character.HumanoidRootPart.Velocity*accomidationfactor))
    else
        placemarker.CFrame = CFrame.new(0, 9999, 0)
    end
    if settings.uwu.autopred == true then
        pingvalue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
        split = string.split(pingvalue,'(')
        ping = tonumber(split[1])
        if ping < 135 then
            predictionValue = 0.1515
        if ping < 130 then
            predictionValue = 0.151
        elseif ping < 125 then
            predictionValue = 0.149
        elseif ping < 120 then
            predictionValue = 0.1468
        elseif ping < 115 then
            predictionValue = 0.1467
        elseif ping < 110 then
            predictionValue = 0.146
        elseif ping < 105 then
            predictionValue = 0.138
        elseif ping < 95 then
            predictionValue = 0.1365
        elseif ping < 90 then
            predictionValue = 0.136
        elseif ping < 85 then
            predictionValue = 0.1345
        elseif ping < 80 then
            predictionValue = 0.134
        elseif ping < 75 then
            predictionValue = 0.1315
        elseif ping < 70 then
            predictionValue = 0.131
        elseif ping < 65 then
            predictionValue = 0.12295
        elseif ping < 60 then
            predictionValue = 0.1229
        elseif ping < 55 then
            predictionValue = 0.12259
        elseif ping < 50 then
            predictionValue = 0.1225
        elseif ping < 45 then
            predictionValue = 0.12565
        elseif ping < 40 then
            predictionValue = 0.1256
        end
    end
end)

local mt = getrawmetatable(game)
local old = mt.__namecall
setreadonly(mt, false)
mt.__namecall = newcclosure(function(...)
    local args = {...}
    if enabled and getnamecallmethod() == "FireServer" and args[2] == "UpdateMousePosI2" and settings.uwu.enabled and plr.Character ~= nil then
        if prediction == true then
            args[3] = plr.Character[selectedPart].Position+(plr.Character[selectedPart].Velocity*predictionValue)
        else
            args[3] = plr.Character[selectedPart].Position
        end
        return old(unpack(args))
    end
    return old(...)
end)

game:GetService("RunService").RenderStepped:Connect(function()
    if settings.uwu.resolver == true and plr.Character ~= nil and enabled and settings.uwu.enabled then
        if settings.uwu.airshot == true and enabled and plr.Character ~= nil then
            if game.Workspace.Players[plr.Name].Humanoid:GetState() == Enum.HumanoidStateType.Freefall then
                if plr.Character ~= nil and plr.Character.HumanoidRootPart.Anchored == true then
                    anchorCount = anchorCount + 1
                    if anchorCount >= maxAnchor then
                        prediction = false
                        wait(2)
                        anchorCount = 0
                    end
                else
                    prediction = true
                    anchorCount = 0
                end
                selectedPart = settings.uwu.falling_hitpart
            else
                if plr.Character ~= nil and plr.Character.HumanoidRootPart.Anchored == true then
                    anchorCount = anchorCount + 1
                    if anchorCount >= maxAnchor then
                        prediction = false
                        wait(2)
                        anchorCount = 0
                    end
                else
                    prediction = true
                    anchorCount = 0
                end
                selectedPart = settings.uwu.airshot_hitpart
            end
        else
            if plr.Character ~= nil and plr.Character.HumanoidRootPart.Anchored == true then
                anchorCount = anchorCount + 1
                if anchorCount >= maxAnchor then
                    prediction = false
                    wait(2)
                    anchorCount = 0
                end
            else
                prediction = true
                anchorCount = 0
            end
            selectedPart = settings.uwu.regular_hitpart
        end
    else
        selectedPart = settings.uwu.regular_hitpart
    end
end)
