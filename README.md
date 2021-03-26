warn('oofminershaven: Loading...')
local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/WhyWKey/uis/main/README.md", true))()
library.options.underlinecolor = "rainbow"
local main = library:CreateWindow("Main")
--

function steal(plr)
    local buy = "buy"
    local pos = "pos"
    local savestring = "savestring"
    local basename = "basename"
    
    local tosteal = workspace.Tycoons:GetChildren()
    for i=1,#tosteal do
        if tosteal[i].Owner.Value == plr then
            basename = tosteal[i].Name
        end
    end
    
    savestring = "local plrbase = game.Players.LocalPlayer.PlayerTycoon.Value\n"
    for i,v in pairs(workspace.Tycoons:findFirstChild(basename):GetChildren()) do
        if v:IsA("Model") then
            local posmaths = tostring(v.Hitbox.CFrame - workspace.Tycoons:findFirstChild(basename).Base.Position)
            pos = tostring('local pos = CFrame.new(' .. posmaths .. ') + plrbase:FindFirstChild("Base").Position' .. '\n')
            buy = tostring('game.ReplicatedStorage.BuyItem:InvokeServer("' .. v.Name .. '", 1)\n')
            savestring = savestring .. pos .. buy .. tostring('game.ReplicatedStorage.PlaceItem:InvokeServer("' .. v.Name ..'", ' .. " pos)" .. '\n\n')
        end
    end
    loadstring(savestring)()
end

function oreboost()
    local factory = nil
    local lava = nil
    local a = game:GetService("Workspace").Tycoons:getDescendants()
    for i=1,#a do
        if a[i].Name == "Owner" then
            if a[i].Value == game:GetService("Players").LocalPlayer.Name then
                factory = a[i].Parent
            end
        end
    end

    local furnace = factory:getDescendants()
    for i=1,#furnace do 
        if furnace[i].Name == "Lava" then 
            lava = furnace[i]
        end
    end

    local upgrade = game:GetService("Workspace").Tycoons:getDescendants()
    for i=1,#upgrade do 
        if upgrade[i].Name == "Upgrade" then 
            local deco = upgrade[i]:getChildren()
            for i=1,#deco do
                if deco[i].Name == "TouchInterest" then
            else
                deco[i]:remove()
            end
        end
        upgrade[i].Size = lava.Size
        upgrade[i].Transparency = 1
        upgrade[i].Position = lava.Position
	    end
	end
end

function loadlayout(lnum)
    local v1 = "Load"
    local v2 = "Layout"..lnum
    local rem = game:GetService("ReplicatedStorage").Layouts
    rem:InvokeServer(v1, v2)
end

function withdraw()
    local rem = game:GetService("ReplicatedStorage").DestroyAll
    rem:InvokeServer()
end

function pulse()
	local rem = game:GetService("ReplicatedStorage").Pulse
    rem:FireServer()
end

function rebirth()
	local rem = game:GetService("ReplicatedStorage").Rebirth
	rem:InvokeServer()
end

function changeradio(id)
	local v1 = id
	local rem = game:GetService("ReplicatedStorage").ChangeRadio
	rem:InvokeServer(v1)
end

function click()
    local rem = game:GetService("ReplicatedStorage").RemoteDrop
    rem:FireServer()
end
--
main:Section('General')
local oreboostbutton = main:Button("Ore Boost", function()
    oreboost()
end)

local pulsebutton = main:Button("Pulse", function()
    pulse()
end)

local withdrawbutton = main:Button("Withdraw All", function()
    withdraw()
end)

local twitchcoinbutton = main:Button("+1 Twitch Coin", function()
    game:GetService("Players").LocalPlayer.TwitchPoints.Value = game.Players.LocalPlayer.TwitchPoints.Value +1
end)

local cloverbutton = main:Button("+1 Clover", function()
    game:GetService("Players").LocalPlayer.Clovers.Value = game.Players.LocalPlayer.Clovers.Value+1
end)    

maskedman = false
local maskedmantoggle = main:Toggle("Masked Man", {flag = maskedmantoggle}, function()
    if maskedman == false then
        maskedman = true
    else
        maskedman = false
    end

    if maskedman == true then
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.EventShop.Visible = true
    else
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.EventShop.Visible = false
    end
end)

enchantui = false
local enchantuitoggle = main:Toggle("Enchant UI", {flag = enchantuitoggle}, function()
    if enchantui == false then
        enchantui = true
    else
        enchantui = false
    end

    if enchantui == true then
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Craftsman.Visible = true
    else
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Craftsman.Visible = false
    end
end)

disablenotifications = false
local disablenotificationstoggle = main:Toggle('Disable Notifications', {flag = 'disablenotificationstoggle'}, function()
    if disablenotifications == false then
        disablenotifications = true
    else
        disablenotifications = false
    end

    if disablenotifications == true then
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Menu.Menu.Sounds.Message.Volume = 0
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Notifications.Visible = false
    else
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Menu.Menu.Sounds.Message.Volume = 0.5
        game:GetService("Players").LocalPlayer.PlayerGui.GUI.Notifications.Visible = true
    end
end)

main:Section("Radio")
selectedradioid = 0
local radioidbox = main:Box('Radio ID', {
    flag = "radioid";
    type = 'number';
}, function(new, old, enter)
    selectedradioid = tonumber(new)
end)

local radioidbutton = main:Button('Play', function()
    changeradio(selectedradioid)
end)

main:Section("Radio 2")
music = {}
local HS = game:GetService('HttpService')
local musicTable = HS:JSONDecode(game:HttpGetAsync('https://epicgameronmylevel696969.000webhostapp.com/Data.json'))
function testContentDeleted(songid)
	local sound = Instance.new("Sound", game:GetService("Lighting"))
	sound.SoundId = "rbxassetid://"..songid
	sound.Volume = 0
	sound:Play()
	wait(0.1)
	if sound.TimeLength < 0.05 then 
		sound:Destroy()		
        return false
	else
		sound:Destroy()		
        return true
	end
end

function updateMusicTable()
	for i,v in pairs(musicTable) do
		if testContentDeleted(musicTable[i].SoundId) == false then
			musicTable[i] = nil
			print(musicTable)
		end
	end
end
updateMusicTable()
for i = 1, #musicTable do 
	if musicTable[i] then 
		table.insert(music, musicTable[i].Name)
	end
end
_G.selectmusic = "idk"
local Music = main:Dropdown("Music", {
    location = _G;
    flag = "selectmusic";
    list = music;
    }, function(new)
    print(_G.selectmusic, "selected.") 
end)
function getSong()
	for i = 1, #musicTable do 
		if musicTable[i] then 
			if musicTable[i].Name == _G.selectmusic then		
				return musicTable[i].SoundId
			end
		end
	end
end
local radioidbutton = main:Button('Play', function()
    changeradio(getSong())
end)
main:Section('Automation')
_G.autorebirth = false
local autorebirthtoggle = main:Toggle('Auto Rebirth', {flag = 'autorebirthtoggle'}, function()
    if _G.autorebirth == false then
        _G.autorebirth = true
    else
        _G.autorebirth = false
    end

    if _G.autorebirth == true then do
        local rebirthcoroutine = coroutine.wrap(function()
            while wait(0.5) do 
                if _G.autorebirth == false then break end
                rebirth()
            end
        end)()
    end
    end
end)

_G.autoclick = false
local autoclicktoggle = main:Toggle('Auto Click', {flag = 'autoclicktoggle'}, function()
    if _G.autoclick == false then
        _G.autoclick = true
    else
        _G.autoclick = false
    end

    if _G.autoclick == true then do
        local autoclickcoroutine = coroutine.wrap(function()
            while wait(0.3) do
                if _G.autoclick == false then break end
                click()
            end
        end)()
    end
    end
end)

_G.autoremote = false
local autoremotetoggle = main:Toggle('Auto Remote', {flag = 'autobuttontoggle'}, function()
    if _G.autoremote == false then
        _G.autoremote = true
    else
        _G.autoclick = false
    end
    
    if _G.autoremote == true then do
        local autoremotecoroutine = coroutine.wrap(function()
            while wait(0.35) do 
                if _G.autoremote == false then break end
                local clickymines = workspace.Tycoons[tostring(game.Players.LocalPlayer.PlayerTycoon.Value)]:GetChildren()
                for i =1, #clickymines do
                    if clickymines[i].ClassName == "Model" then
                        if clickymines[i].Model:findFirstChild("Button") then
                            local de = clickymines[i].Model:GetChildren()
                            for i =1, #de do
                                if de[i].Name == "Button" then
                                    game.ReplicatedStorage.Click:FireServer(de[i])
                                end
                            end
                        end
                    end
                end
            end
        end)()
    end
    end
end)

_G.autooreboost = false
local autooreboosttoggle = main:Toggle('Auto Ore Boost', {flag = 'autooreboosttoggle'}, function()
    if _G.autooreboost == false then
        _G.autooreboost = true
    else
        _G.autooreboost = false
    end

    if _G.autooreboost == true then
        local autooreboostcoroutine = coroutine.wrap(function()
            while wait(1) do
                if _G.autooreboost == false then break end
                oreboost()
            end
        end)()
    end
end)

main:Section('Auto Layout')
_G.autolayout = false
selectedlayout = 1
local autolayoutbox = main:Box('Select Layout', {
    flag = "selectedlayout";
    type = 'number';
    min = 1;
    max = 4;
}, function(new, old, enter)
    selectedlayout = tonumber(new)
end)

local autolayouttoggle = main:Toggle('Auto Layout', {flag = 'autolayouttoggle'}, function()
    if _G.autolayout == false then
        _G.autolayout = true
    else
        _G.autolayout = false
    end

    if _G.autolayout == true then do
        local autolayoutcoroutine = coroutine.wrap(function()
            while wait(0.25) do
                if _G.autolayout == false then break end
                loadlayout(selectedlayout)
            end
        end)()
    end
    end
end)

main:Section("Base Stealer")
local plrarray = game:GetService("Players"):GetPlayers() 
local Players = game:GetService("Players")
list1 = {}

for i=1,#plrarray do
table.insert(list1, plrarray[i].Name)
end

Players.PlayerAdded:Connect(function(player)
    table.insert(list1, player.Name)
    print(player, "has joined and added to the Dropdown")
end)

Players.PlayerRemoving:Connect(function(player) 
    for i = 1, #list1 do
	    if list1[i] == player then
		    print(list1[i])
		    table.remove(list1, i)
	     	print(list1[i])
	    end
    end	
end)

local PlayerList = main:Dropdown("Players", {
    location = _G;
    flag = "selectplr";
    list = list1;
    }, function(new)
    print(_G.selectplr, "selected.") 
end)

local stealbutton = main:Button("Steal", function()
    withdraw()
    steal(_G.selectplr)
end)

print('oofminershaven: Loaded!')
