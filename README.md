local game = Instance.new("Game")
game.Name = "Blox Fruits Sea 3 Simulator"
game.Workspace.Gravity = 196.2
game.Workspace.Map = game.Workspace:FindFirstChild("ThirdSeaMap")

local Fruits = {
    Bomb = {Damage = 50, Cooldown = 5},
    Rubber = {Damage = 55, Cooldown = 6},
    Flame = {Damage = 60, Cooldown = 7},
    Phoenix = {Damage = 70, Cooldown = 8},
    Dark = {Damage = 75, Cooldown = 9}
}

local Races = {
    Human = {Speed = 16, Strength = 20},
    Shark = {Speed = 18, Strength = 25},
    Fishman = {Speed = 20, Strength = 30}
}

local Bosses = {
    ["Gorilla King"] = {Health = 6000, Damage = 120},
    ["Dark Master"] = {Health = 8000, Damage = 150},
    ["Phoenix General"] = {Health = 7000, Damage = 140}
}

local Weapons = {
    Saber = {Damage = 60, Speed = 1.1},
    DarkBlade = {Damage = 90, Speed = 1.3},
    FlameCannon = {Damage = 70, Speed = 1.2}
}

local Haki = {
    Observation = {Vision = 60},
    Armament = {Defense = 50}
}

local AdminCommands = {
    ["!kick"] = function(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            player:Kick("Kicked by Admin")
        end
    end,

    ["!ban"] = function(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            player:Kick("Banned by Admin")
        end
    end,

    ["!tp"] = function(playerName, x, y, z)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            player.Character:MoveTo(Vector3.new(x, y, z))
        end
    end,

    ["!givefruit"] = function(playerName, fruitName)
        local player = game.Players:FindFirstChild(playerName)
        if player and Fruits[fruitName] then
            local fruit = Fruits[fruitName]
            local fruitTool = Instance.new("Tool")
            fruitTool.Name = fruitName
            fruitTool.Parent = player.Backpack
        end
    end,

    ["!heal"] = function(playerName)
        local player = game.Players:FindFirstChild(playerName)
        if player then
            player.Character.Humanoid.Health = player.Character.Humanoid.MaxHealth
        end
    end,

    ["!setrace"] = function(playerName, raceName)
        local player = game.Players:FindFirstChild(playerName)
        if player and Races[raceName] then
            player:SetAttribute("Race", raceName)
        end
    end
}

game.Players.PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        local command, args = msg:match("!(%w+)%s*(.*)")
        if command and AdminCommands[command] then
            local splitArgs = args:split(" ")
            AdminCommands[command](player.Name, unpack(splitArgs))
        end
    end)
end)

function SetupWeapons(player)
    local currentWeapons = player:FindFirstChild("Weapons")
    if not currentWeapons then
        local weaponFolder = Instance.new("Folder")
        weaponFolder.Name = "Weapons"
        weaponFolder.Parent = player
    end

    for weaponName, stats in pairs(Weapons) do
        local tool = Instance.new("Tool")
        tool.Name = weaponName
        tool.Parent = player:FindFirstChild("Weapons")
    end
end

game.Players.PlayerAdded:Connect(function(player)
    SetupWeapons(player)
end)

function EquipHaki(player, hakiType)
    local haki = Haki[hakiType]
    if haki then
        player:SetAttribute("Haki", hakiType)
    end
end

game.Players.PlayerAdded:Connect(function(player)
    player.Chatted:Connect(function(msg)
        local hakiCommand, hakiArgs = msg:match("!(%w+)%s*(.*)")
        if hakiCommand == "haki" then
            EquipHaki(player, hakiArgs)
        end
    end)
end)

print("Blox Fruits Sea 3 Setup Complete!")
