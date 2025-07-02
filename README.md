-- Bloqueio de execução múltipla universal para Roblox
local scriptId = "MasterShukakuHub" -- Troque esse nome para um identificador único se quiser permitir múltiplos scripts distintos

if _G["executou_"..scriptId] then
    pcall(function()
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Erro";
            Text = "Este script já foi executado e não pode ser executado novamente!";
            Duration = 5;
        })
    end)
    return -- Para o script aqui
else
    _G["executou_"..scriptId] = true
end

loadstring(game:HttpGet("https://pastebin.com/raw/d6B4BhyA", true))() -- Abertura

loadstring(game:HttpGet("https://raw.githubusercontent.com/DudxJs/GUI/main/GUI.lua"))()

local gui = _G.DudxJsGUI:New("MasterShukaku Hub", "rbxassetid://92433947031436")

-- ==================
--  ⬇️Tab Buttons⬇️
-- ==================

local House = gui:AddTab("House")
local Avatar = gui:AddTab("Avatar")
local Car = gui:AddTab("Car")
local Fun = gui:AddTab("Fun")
local Itens = gui:AddTab("Build")
local Others = gui:AddTab("Others")
local Teleportes = gui:AddTab("Teleportes")
local Misc = gui:AddTab("Misc")
local Kill = gui:AddTab("Kill")
local Tools = gui:AddTab("Tools")
local Premium = gui:AddTab("Premium")
local Map = gui:AddTab("Map")
local Visual = gui:AddTab("Visual")
local Scripts = gui:AddTab("Scripts")

-- ==================================
--  ⬇️ RP Nome/Bio Inicialização ⬇️
-- ==================================

-- Services
local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Remotes
local RPNameColorRemote = ReplicatedStorage.RE["1RPNam1eColo1r"]
local RPBioColorRemote = ReplicatedStorage.RE["1RPNam1eColo1r"]
local RPNameTextRemote = ReplicatedStorage.RE["1RPNam1eTex1t"]
local RPBioTextRemote = ReplicatedStorage.RE["1RPNam1eTex1t"]

-- Altere os valores abaixo se quiser mudar nome, bio ou cores
local rpName = "M\210\137A\210\137S\210\137T\210\137E\210\137R\210\137  S\210\137H\210\137U\210\137K\210\137A\210\137K\210\137U\210\137"
local rpBio = "\227\128\178Script Feito Por: Dudx_js\227\128\134"

local rpNameColor = Color3.fromRGB(163, 2, 0)  -- Ajuste conforme desejado
local rpBioColor = Color3.fromRGB(0, 0, 0)     -- Ajuste conforme desejado

-- Define a cor do nome RP
RPNameColorRemote:FireServer("PickingRPNameColor", rpNameColor)

-- Define a cor da bio RP
RPBioColorRemote:FireServer("PickingRPBioColor", rpBioColor)

-- Define o texto do nome RP
RPNameTextRemote:FireServer("RolePlayName", rpName)

-- Define o texto da bio RP
RPBioTextRemote:FireServer("RolePlayBio", rpBio)

-- ====================
--  ⬇️House Buttons⬇️
-- ====================

House:AddLabel("House Bio")

House:AddInput("Bio Text", "", function(value)
args = {
        [1] = "BusinessName",
        [2] = value
    }
    game:GetService("ReplicatedStorage").RE["1RPHous1eEven1t"]:FireServer(unpack(args))
end)

local rainbowConnection
local running = false

House:AddSwitch("Rainbow House Bio", function(state)
    if state then
        running = true
        rainbowConnection = task.spawn(function()
            while running do
                local success, errorMessage = pcall(function()
                    local players = game:GetService("Players")
                    local replicatedStorage = game:GetService("ReplicatedStorage")
                    local localPlayer = players.LocalPlayer
                    local colorEvent = replicatedStorage:WaitForChild("RE"):WaitForChild("1RPHous1eEven1tColo1r")
                    local colorFrames = localPlayer.PlayerGui.MainGUIHandler.HouseControl.HomeBus.HomeText.Picks.Frame:GetChildren()
                    local numColors = #colorFrames
                    
                    for i = 1, numColors do
                        if not running then break end -- parada segura no meio do processo
                        local colorFrame = colorFrames[i]
                        if colorFrame and colorFrame:IsA("GuiObject") and colorFrame:FindFirstChild("Color") then
                             args = {
                                [1] = "PickingBusinessNameColor",
                                [2] = colorFrame.Color
                            }
                            colorEvent:FireServer(unpack(args))
                            task.wait(0.7)
                        end
                    end
                    
                    task.wait(1)
                end)

                if not success then
                    warn("Error in Rainbow House Bio loop:", errorMessage)
                    task.wait(5)
                end
            end
        end)
    else
        running = false
    end
end)

House:AddLabel("House Design")

local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Remote = ReplicatedStorage.RE:FindFirstChild("1Player1sHous1e")
local runningRainbow = false

-- Duração da transição em segundos
local duration = 3 

-- Função para gerar uma cor aleatória suave (menos saturada)
local function getRandomColor()
    local r = math.random(50, 255) / 255
    local g = math.random(50, 255) / 255
    local b = math.random(50, 255) / 255
    return Color3.new(r, g, b)
end

-- Interpolação linear entre duas cores (lerp)
local function lerpColor(a, b, t)
    local r = a.R + (b.R - a.R) * t
    local g = a.G + (b.G - a.G) * t
    local b = a.B + (b.B - a.B) * t
    return Color3.new(r, g, b)
end

local function smoothColorTransition()
    local currentColor = getRandomColor()
    local targetColor = getRandomColor()

    while runningRainbow do
        local startTime = tick()
        while tick() - startTime < duration do
            if not runningRainbow then return end
            local elapsed = tick() - startTime
            local alpha = elapsed / duration
            local newColor = lerpColor(currentColor, targetColor, alpha)

            Remote:FireServer("ColorPickHouse", newColor)

            task.wait(0.1)
        end

        currentColor = targetColor
        targetColor = getRandomColor()
    end
end

-- Assumindo que House é um objeto que cria toggles na sua UI
House:AddSwitch("Rainbow House", function(state)
    runningRainbow = state
    if state then
        task.spawn(smoothColorTransition)
    end
end)

House:AddLabel("House Settings")

local SelectHouse
local SelectedPlayerName

 function GetExistentHouses()
	local houseTable = {}
	for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
		if string.find(v.Name, "House") and v:FindFirstChild("HousePickedByPlayer") then
			table.insert(houseTable, {
				FullName = v.Owner.Value,
				HouseNumber = v.Number.Number.Value,
				Model = v
			})
		end
	end
	return houseTable
end

 function FindHouseByPartialName(partialName)
	partialName = partialName:lower()
	for _, data in pairs(GetExistentHouses()) do
		if string.find(data.FullName:lower(), partialName) then
			return data
		end
	end
end

House:AddInput("Target Player", "", function(txt)
 		local result = FindHouseByPartialName(txt)
		if result then
			SelectHouse = result.HouseNumber
			SelectedPlayerName = result.FullName
			warn("[+]", "Found: " .. SelectedPlayerName .. " - House #" .. SelectHouse)
		else
			SelectHouse = nil
			SelectedPlayerName = nil
			warn("[-]", "No matching house found")
		end
	end)

House:AddButton("Teleport to the house", function()
		if not SelectHouse then return end
		for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
			if v.Name ~= "DontDelete" and v.Number.Number.Value == SelectHouse then
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(v.WorldPivot.Position)
				break
			end
		end
	end)

House:AddButton("Teleport to the vault", function()
		if not SelectHouse then return end
		for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
			if v.Name ~= "DontDelete" and v.Number.Number.Value == SelectHouse then
				local safe = v.HousePickedByPlayer.HouseModel["001_Safe"]
				game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(safe.WorldPivot.Position)
				break
			end
		end
	end)

House:AddSwitch("Noclip through the house door", function(Value)
		if not SelectHouse then return end
		for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
			if v.Name ~= "DontDelete" and v.Number.Number.Value == SelectHouse then
				pcall(function()
					local doors = v.HousePickedByPlayer.HouseModel["001_HouseDoors"].HouseDoorFront:GetChildren()
					for _, part in pairs(doors) do
						if part:IsA("BasePart") then
							part.CanCollide = not Value
						end
					end
				end)
				break
			end
		end
	end)

House:AddSwitch("Spam Bell", function(Value)
		getgenv().ChaosHubAutoSpawnDoorbellValue = Value
		task.spawn(function()
			while getgenv().ChaosHubAutoSpawnDoorbellValue do
				if not SelectHouse then break end
				for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
					if v.Name ~= "DontDelete" and v.Number.Number.Value == SelectHouse then
						fireclickdetector(v.HousePickedByPlayer.HouseModel["001_DoorBell"].TouchBell.ClickDetector)
						break
					end
				end
				task.wait(0.5)
			end
		end)
	end)

House:AddSwitch("Spam Knock", function(Value)
		getgenv().ShnmaxAutoSpawnDoorValue = Value
		task.spawn(function()
			while getgenv().ShnmaxAutoSpawnDoorValue do
				if not SelectHouse then break end
				for _, v in pairs(workspace["001_Lots"]:GetChildren()) do
					if v.Name ~= "DontDelete" and v.Number.Number.Value == SelectHouse then
						fireclickdetector(v.HousePickedByPlayer.HouseModel["001_HouseDoors"].HouseDoorFront.Knock.TouchBell.ClickDetector)
						break
					end
				end
				task.wait(0.5)
			end
		end)
	end)

House:AddLabel("House Settings Player")

House:AddButton("House Fire", function()
 args = {
    [1] = "PlayerWantsFireOnFirePassNotShowingAnyone"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e"):FireServer(unpack(args))
  	end)

local loopTravaPorta = false
local Remote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e")

local function portaLoop()
    while loopTravaPorta do
         args = { [1] = "LockDoors" }
        Remote:FireServer(unpack(args))
        wait(1.5) -- intervalo entre trava/destrava (ajustável)
    end
end

House:AddSwitch("Spam Door Lock", function(state)
    loopTravaPorta = state
    if state then
        spawn(portaLoop)
    end
end)

local poolLoopAtivo = false
local Remote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e")

local function poolLoop()
    while poolLoopAtivo do
         args = { [1] = "PoolOnOff" }
        Remote:FireServer(unpack(args))
        wait(2) -- tempo entre liga/desliga (ajustável)
    end
end

House:AddSwitch("Spam Pool", function(state)
    poolLoopAtivo = state
    if state then
        spawn(poolLoop)
    end
end)

local runningGarage = false
House:AddSwitch("Spam Garage", function(state)
    runningGarage = state
    if state then
        spawn(function()
            while runningGarage do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e"):FireServer("GarageDoor")
                wait(0.5)
            end
        end)
        print("Loop Garagem ON")
    else
        print("Loop Garagem OFF")
    end
end)

-- Loop Cortinas
local runningCurtains = false
House:AddSwitch("Spam Curtains", function(state)
    runningCurtains = state
    if state then
        spawn(function()
            while runningCurtains do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e"):FireServer("Curtains")
                wait(0.5)
            end
        end)
        print("Loop Cortinas ON")
    else
        print("Loop Cortinas OFF")
    end
end)

local runningBaby = false
House:AddSwitch("Spam Baby Stuff", function(state)
    runningBaby = state
    if state then
        spawn(function()
            while runningBaby do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e"):FireServer("BabyOptionYes")
                wait(0.5)
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sHous1e"):FireServer("BabyOptionNo")
                wait(0.5)
            end
        end)
        print("Loop Bebês ON")
    else
        print("Loop Bebês OFF")
    end
end)

House:AddButton("AntiBlock - UnbanHouse", function()
 loadstring(game:HttpGet('https://pastebin.com/raw/17dSEXCP'))()
end)

-- =====================
--  ⬇️Avatar Buttons⬇️
-- =====================

Avatar:AddLabel("Normal Name")
Avatar:AddInput("RP Name", "", function(value)
 args = {
[1] = "RolePlayName", [2] = value

}
game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
    end)

local trocandoCores = false

Avatar:AddButton("Rainbow Name", function()
    if trocandoCores then return end
    trocandoCores = true

    task.spawn(function()
        while trocandoCores do
            -- Gira o hue com base no tempo para obter cores sempre diferentes e vivas
            local hue = (tick() % 10) / 10
            local color = Color3.fromHSV(hue, 1, 1)

            local args = {
                [1] = "PickingRPNameColor",
                [2] = color
            }

            local remote = game:GetService("ReplicatedStorage"):FindFirstChild("RE"):FindFirstChild("1RPNam1eColo1r")
            if remote then
                remote:FireServer(unpack(args))
            end

            task.wait(0.15)
        end
    end)
end)

Avatar:AddButton("Off Rainbow Name", function()
    trocandoCores = false
end)

Avatar:AddLabel("Normal Bio")
Avatar:AddInput("RP Bio", "", function(value)
    local args = {
        [1] = "RolePlayBio",
        [2] = value
    }
    local event = game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t")
    if event then
        event:FireServer(unpack(args))
    else
        warn("Evento não encontrado!")
    end
end)

Avatar:AddButton("Rainbow Bio", function()
    if trocandoCores then return end
    trocandoCores = true

    task.spawn(function()
        while trocandoCores do
            -- Gira o hue com base no tempo para obter cores sempre diferentes e vivas
            local hue = (tick() % 10) / 10
            local color = Color3.fromHSV(hue, 1, 1)

            local args = {
                [1] = "PickingRPBioColor",
                [2] = color
            }

            local remote = game:GetService("ReplicatedStorage"):FindFirstChild("RE"):FindFirstChild("1RPNam1eColo1r")
            if remote then
                remote:FireServer(unpack(args))
            end

            task.wait(0.15)
        end
    end)
end)

Avatar:AddButton("Off Rainbow Bio", function()
    trocandoCores = false
end)

Avatar:AddLabel("FE Avatar Copier")

local P, R, LP = game:GetService("Players"), game:GetService("ReplicatedStorage"), game:GetService("Players").LocalPlayer
local tgt, last, mode = nil, nil, "Brookhaven"

local function W(id) pcall(function() R.Remotes.Wear:InvokeServer(tonumber(id)) end) end

local function RB()
	do R.Remotes.ChangeCharacterBody:InvokeServer({0,0,0,0,0,0}, "AllBlocky") end
end

local function ST(uid)
	do
		local s, r = pcall(function() return P:GetCharacterAppearanceInfoAsync(uid) end)
		if s and r.bodyColors and r.bodyColors.HeadColor then
			R.Remotes.ChangeBodyColor:FireServer(tostring(r.bodyColors.HeadColor))
		end
	end
end

local function CC(d)
	do
		for _, i in ipairs{d.Shirt, d.Pants, d.GraphicTShirt, d.Face} do
			if tonumber(i) and i ~= 0 then W(i) task.wait(0.1) end
		end
	end
end

local function CA(d)
	do
		pcall(function()
			for _, v in ipairs(d:GetAccessories(true)) do
				if v.AssetId then W(v.AssetId) task.wait(0.1) end
			end
		end)
	end
end

local function CB(d, src)
	do
		local p = {
			tonumber(d.Torso), tonumber(d.RightArm), tonumber(d.LeftArm),
			tonumber(d.RightLeg), tonumber(d.LeftLeg), tonumber(d.Head)
		}
		R.Remotes.ChangeCharacterBody:InvokeServer(p, src)
	end
end

local function CAn(d)
	do
		for _, i in ipairs{
			d.IdleAnimation, d.WalkAnimation, d.RunAnimation,
			d.JumpAnimation, d.FallAnimation, d.SwimAnimation
		} do
			if tonumber(i) and i ~= 0 then W(i) task.wait(0.1) end
		end
	end
end

local function CopyB(p)
	do
		local h = p.Character and p.Character:FindFirstChildOfClass("Humanoid")
		if not h then return end
		local d = h:GetAppliedDescription()
		local b = p.Character:FindFirstChildOfClass("BodyColors")
		if b and b.HeadColor then
			R.Remotes.ChangeBodyColor:FireServer(tostring(b.HeadColor))
		end
		pcall(function()
			CA(LP.Character:FindFirstChildOfClass("Humanoid"):GetAppliedDescription())
			RB() CA(d) task.wait(0.1)
			CB(d, "ShnmaxHub") CC(d) CAn(d)
		end)
	end
end

local function CopyO(uid)
	do
		pcall(function()
			local d = P:GetHumanoidDescriptionFromUserId(uid)
			CA(LP.Character:FindFirstChildOfClass("Humanoid"):GetAppliedDescription())
			RB() CA(d) task.wait(0.1)
			CB(d, "ShnmaxHub") CC(d) CAn(d)
			ST(uid)
		end)
	end
end

local function Find(name)
	if not name or name == "" then return end
	name = name:lower()
	local res = {}
	for _, p in ipairs(P:GetPlayers()) do
		if p ~= LP and p.Name:lower():find(name, 1, true) then
			table.insert(res, p)
		end
	end
	table.sort(res, function(a, b)
		local sa, sb = a.Name:lower():sub(1, #name) == name, b.Name:lower():sub(1, #name) == name
		if sa and not sb then return true end
		if not sa and sb then return false end
		return #a.Name < #b.Name
	end)
	return res[1]
end

Avatar:AddInput("Target Player", "type name..", function(txt)
	if txt == "" then if last then txt = last else return end end
	local p = Find(txt)
	if p then tgt, last = p, p.Name end
end)

Avatar:AddDropdown("Copy Meathod", {"Brookhaven", "Original Avatar"}, function(opt)
	mode = opt
end)

Avatar:AddButton("Copy Avatar", function()
	if not tgt then return end
	if mode == "Brookhaven" then CopyB(tgt) else CopyO(tgt.UserId) end
end)

Avatar:AddButton("Copy Avatar Nearest", function()
	local c, d = nil, math.huge
	for _, p in pairs(P:GetPlayers()) do
		if p ~= LP and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
			local m = (p.Character.HumanoidRootPart.Position - LP.Character.HumanoidRootPart.Position).Magnitude
			if m < d then c, d = p, m end
		end
	end
	if c then if mode == "Brookhaven" then CopyB(c) else CopyO(c.UserId) end end
end)

Avatar:AddButton("Copy Avatar Random", function()
	local list = {}
	for _, p in pairs(P:GetPlayers()) do
		if p ~= LP then table.insert(list, p) end
	end
	if #list > 0 then
		local r = list[math.random(1, #list)]
		if mode == "Brookhaven" then CopyB(r) else CopyO(r.UserId) end
	end
end)
    
Avatar:AddLabel("Avatar")

Avatar:AddButton("Old R6 Animation", function()
 loadstring(game:HttpGet('https://gist.githubusercontent.com/Imperador950/f9e54330eb4a92331204aae37ec11aef/raw/db18d1c348beb8a79931346597137518966f2188/Byshelby'))()
end)

Avatar:AddButton("Old R15 Animation", function()
local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Parar e remover todas as animações ativas
for _, track in ipairs(humanoid:GetPlayingAnimationTracks()) do
track:Stop()
track:Destroy()
end

-- Remover objetos Animation de dentro do personagem
for _, descendant in ipairs(character:GetDescendants()) do
if descendant:IsA("Animation") then
descendant:Destroy()
end
end

-- Forçar remoção de AnimationId em objetos suspeitos
for _, obj in ipairs(character:GetDescendants()) do
if obj:IsA("Tool") or obj:IsA("LocalScript") or obj:IsA("ModuleScript") then
for _, child in ipairs(obj:GetDescendants()) do
if child:IsA("Animation") then
child:Destroy()
end
end
end
end
end)

Avatar:AddLabel("Job Section")

local ChangeJobLoopEnabled = false

local function changeJobLoop()
while ChangeJobLoopEnabled do
args = {
[1] = "Dancer"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Grocery Store"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Police"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Hospital"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Fire House"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Teacher"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Student"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "Bank"
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
wait(0,5)
args = {
[1] = "S.W.A.T."
}

game:GetService("ReplicatedStorage").Remotes.GiveJobUIMenu:FireServer(unpack(args))
end
end

Avatar:AddSwitch("Loop Jobs", function(Value)
if Value then
ChangeJobLoopEnabled = true
changeJobLoop()
else
ChangeJobLoopEnabled = false
end
end)

Avatar:AddLabel("Character Settings")

local StarterGui = game:GetService("StarterGui")
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local function getHumanoid()
character = player.Character or player.CharacterAdded:Wait()
return character:FindFirstChildOfClass("Humanoid")
end

local function notify(title, text, duration)
StarterGui:SetCore("SendNotification", {
Title = title,
Text = text,
Duration = duration or 3
})
end

Avatar:AddInput("Speed", "", function(text)
local value = tonumber(text)
local humanoid = getHumanoid()
if humanoid and value then
humanoid.WalkSpeed = value
notify("Velocidade Alterada", "Nova velocidade: " .. value)
else
warn("Valor inválido para velocidade")
notify("Erro", "Valor inválido para velocidade")
end
end)

Avatar:AddInput("Jump", "", function(text)
local value = tonumber(text)
local humanoid = getHumanoid()
if humanoid and value then
humanoid.JumpPower = value
notify("Salto Alterado", "Novo salto: " .. value)
else
warn("Valor inválido para salto")
notify("Erro", "Valor inválido para salto")
end
end)

Avatar:AddButton("Reset Walkspeed/Jumppower", function()
local humanoid = getHumanoid()
if humanoid then
humanoid.WalkSpeed = 16
humanoid.JumpPower = 50
notify("Valores Restaurados", "Velocidade e salto padrão aplicados.")
else
warn("Humanoid não encontrado!")
notify("Erro", "Humanoid não encontrado!")
end
end)

Avatar:AddButton("Refresh Avatar", function()
game:GetService("ReplicatedStorage").Remotes.ResetCharacterAppearance:FireServer()
end)

Avatar:AddButton("Reset 1", function()
local character = game.Players.LocalPlayer.Character
local humanoid = character:FindFirstChild("Humanoid")

for _, part in pairs(character:GetChildren()) do  
	if part:IsA("Part") or part:IsA("BasePart") or part:IsA("MeshPart") then  
		if part.Anchored then  
			part.Anchored = false  
		end  
	end  
end  

if humanoid then  
	humanoid.Health = 0  
end

end)

Avatar:AddButton("Reset 2", function()
local character = game.Players.LocalPlayer.Character
if not character then return end

for _, part in pairs(character:GetChildren()) do  
	if part:IsA("Part") or part:IsA("BasePart") or part:IsA("MeshPart") then  
		if part.Anchored then  
			part.Anchored = false  
		end  
	end  
end  

local upperTorso = character:FindFirstChild("UpperTorso")  
if upperTorso then  
	upperTorso:Destroy()  
end

end)

Avatar:AddLabel("Character Appearance")

Avatar:AddButton("Fire Avatar", function()
local player = game.Players.LocalPlayer
local character = player.Character
if not character then return end

local house = workspace:FindFirstChild("001_Lots") and workspace["001_Lots"]:FindFirstChild(player.Name .. "House")  
local mall = house  
	and house:FindFirstChild("HousePickedByPlayer")  
	and house.HousePickedByPlayer:FindFirstChild("HouseModel")  
	and house.HousePickedByPlayer.HouseModel:FindFirstChild("001_BBQ")  
	and house.HousePickedByPlayer.HouseModel["001_BBQ"]:FindFirstChild("CatchFire")  

local hrp = character:FindFirstChild("HumanoidRootPart")  

if mall and hrp then  
	firetouchinterest(hrp, mall, 0)  
	task.wait()  
	firetouchinterest(hrp, mall, 1)  
end

end)

local FireLoopAtivo = false

local function toggleFireLoop(ativo)
FireLoopAtivo = ativo
notify("Loop Fire", ativo and "Ativado" or "Desativado")

if ativo then  
    task.spawn(function()  
        while FireLoopAtivo do  
            local house = Workspace:FindFirstChild("001_Lots") and Workspace["001_Lots"]:FindFirstChild(player.Name .. "House")  
            local mall = house and house:FindFirstChild("HousePickedByPlayer") and house.HousePickedByPlayer:FindFirstChild("HouseModel"):FindFirstChild("001_BBQ"):FindFirstChild("CatchFire")  
            local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")  
            if mall and hrp then  
                firetouchinterest(hrp, mall, 0)  
                task.wait()  
                firetouchinterest(hrp, mall, 1)  
            end  
            task.wait(1)  
        end  
    end)  
end

end

Avatar:AddSwitch("Loop Fire Avatar", function(state)
toggleFireLoop(state)
end)

Avatar:AddButton("Smoke Avatar", function()
local mall = Workspace:FindFirstChild("WorkspaceCom") and Workspace.WorkspaceCom:FindFirstChild("001_Mall")
mall = mall and mall:FindFirstChild("001_Pizza") and mall["001_Pizza"]:FindFirstChild("CatchFire")
local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
if mall and hrp then
firetouchinterest(hrp, mall, 0)
task.wait()
firetouchinterest(hrp, mall, 1)
end
end)

local SmokeLoopAtivo = false

function toggleSmokeLoop(ativo)
SmokeLoopAtivo = ativo
notify("Loop Smoke", ativo and "Ativado" or "Desativado")

if ativo then  
    task.spawn(function()  
        while SmokeLoopAtivo do  
            local mall = Workspace:FindFirstChild("WorkspaceCom") and Workspace.WorkspaceCom:FindFirstChild("001_Mall")  
            mall = mall and mall:FindFirstChild("001_Pizza") and mall["001_Pizza"]:FindFirstChild("CatchFire")  
            local hrp = player.Character and player.Character:FindFirstChild("HumanoidRootPart")  
            if mall and hrp then  
                firetouchinterest(hrp, mall, 0)  
                task.wait()  
                firetouchinterest(hrp, mall, 1)  
            end  
            task.wait(1)  
        end  
    end)  
end

end

Avatar:AddSwitch("Loop Smoke Avatar", function(state)
toggleSmokeLoop(state)
end)

Avatar:AddLabel("Body Section")
Avatar:AddButton("FE Faceless", function()
local head = game.Players.LocalPlayer.Character:WaitForChild("Head")
local face = head:FindFirstChildOfClass("Decal")
if face then
face.Texture = "rbxassetid://0"
end
end)

Avatar:AddButton("FE Naked", function()
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local shirt = character:FindFirstChildOfClass("Shirt")  
if shirt then shirt:Destroy() end  

local pants = character:FindFirstChildOfClass("Pants")  
if pants then pants:Destroy() end

end)

Avatar:AddButton("Refresh Avatar", function()
game:GetService("ReplicatedStorage").Remotes.ResetCharacterAppearance:FireServer()
end)

Avatar:AddLabel("Custom Hats, Hairs, and some...")

Avatar:AddInput("Id Box", "", function(Value)
args = {
[1] = Value
}

game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
end)

Avatar:AddButton("Refresh Avatar", function()
game:GetService("ReplicatedStorage").Remotes.ResetCharacterAppearance:FireServer()
end)

-- ==================
--  ⬇️Car Buttons⬇️
-- ==================

-- URL para carregar a playlist
local playlistUrl = "https://pastebin.com/raw/uDreR61A"

-- Carregar a playlist dinamicamente
local PlayList = loadstring(game:HttpGet(playlistUrl))()

-- Verificar se a playlist foi carregada corretamente
if PlayList then
    print("Playlist carregada com sucesso!")
else
    error("Erro ao carregar a playlist!")
end

-- Função para tocar música no carro e na moto/scooter
local function playMusic(musicId)
    if musicId and musicId ~= "" then
        -- Carro
        local argsCar = {
            [1] = "PickingCarMusicText", -- Customize com o evento correto
            [2] = musicId
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(argsCar))
        print("Tocando música no carro com ID:", musicId)

        -- Moto/Scooter
        local Argument1 = game:GetService("ReplicatedStorage").RE["1NoMoto1rVehicle1s"]
        Argument1:FireServer(
            "PickingScooterMusicText",
            musicId -- agora também usa o mesmo musicId informado
        )
        print("Tocando música na moto/scooter com ID:", musicId)
    else
        print("Por favor, insira um ID de música válido.")
    end
end
-- Variável para armazenar o último ID de música selecionado ou digitado
local lastMusicId = nil

-- Aba Música

-- Label informativo
Car:AddLabel("Music Car ID『Gamepass』")

-- Input para digitar o ID da música
Car:AddInput("Digite o ID da Música", "Ex.: 12345678", function(id)
    local musicId = tonumber(id) -- Converter para número
    if musicId then
        lastMusicId = id -- Atualizar o último ID
        print("ID da Música atualizado pelo Input:", lastMusicId)
    else
        print("ID inválido. Por favor, insira um número.")
    end
end)

-- Dropdown para selecionar uma música da PlayList
local musicOptions = {}
for _, song in pairs(PlayList) do
    table.insert(musicOptions, song.Name) -- Adiciona somente o nome ao Dropdown
end

Car:AddDropdown("Escolha uma música", musicOptions, function(selectedName)
    for _, song in pairs(PlayList) do
        if selectedName == song.Name then
            lastMusicId = song.ID -- Atualizar o último ID
            print("ID da Música atualizado pelo Dropdown:", lastMusicId)
            break
        end
    end
end)

-- Botão para tocar música
Car:AddButton("Tocar Música", function()
    if lastMusicId then
        playMusic(lastMusicId) -- Tocar música com o último ID
    else
        print("Nenhum ID de música foi selecionado ou digitado.")
    end
end)

Car:AddLabel("Target Car Player")

local Vehicles = workspace:FindFirstChild("Vehicles")
local Player = game.Players.LocalPlayer
local SelectCar

local Tabela = {}
if Vehicles then
    for _, car in ipairs(Vehicles:GetChildren()) do
        if car:IsA("Model") then
            table.insert(Tabela, car.Name)
        end
    end
end

local CarDropdown = Car:AddDropdown("Vehicle - ", Tabela, function(Value)
        SelectCar = Value
    end)

Car:AddButton("Teleport to Vehicle", function()
local Car = Vehicles:FindFirstChild(tostring(SelectCar))
  if Car then
    for _, basepart in ipairs(Car:GetDescendants()) do
      if basepart:IsA("BasePart") then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(basepart.Position)
      end
    end
  end
end)

Car:AddButton("Teleport to Seat", function()
    local Car = Vehicles:FindFirstChild(tostring(SelectCar))
    local Character = Player.Character
    if Car and Character then
        local Seat = Car:FindFirstChild("Body") and Car.Body:FindFirstChild("VehicleSeat") or Car:FindFirstChildWhichIsA("VehicleSeat")
        local RootPart = Character:FindFirstChild("HumanoidRootPart")

        if Seat and RootPart then
            RootPart.CFrame = Seat.CFrame + Vector3.new(0, 3, 0)
        end
    end
end)

Car:AddButton("Pull Vehicle", function()
        local Car = Vehicles:FindFirstChild(tostring(SelectCar))
        local Character = Player.Character
        if Car and Character then
            local Seat = Car:FindFirstChild("Body") and Car.Body:FindFirstChild("VehicleSeat")
            local Humanoid = Character:FindFirstChildOfClass("Humanoid")
            local RootPart = Character:FindFirstChild("HumanoidRootPart")

            if Seat and Humanoid and RootPart then
                if not Car.PrimaryPart then
                    local try = Car:FindFirstChild("Primary") or Car:FindFirstChildWhichIsA("BasePart")
                    if try then
                        Car.PrimaryPart = try
                    end
                end

                if Car.PrimaryPart then
                    local OldPos = RootPart.CFrame

                    repeat
                        task.wait()
                        RootPart.CFrame = Seat.CFrame
                    until Humanoid.Sit

                    task.wait(0.7)

                    Car:SetPrimaryPartCFrame(OldPos)
                end
            end
        end
    end)
    
Car:AddLabel("Tank Section")

local player = game:GetService("Players").LocalPlayer
local spamConns = {}

local function explode(car)
    local p = car:FindFirstChild("Body") and car.Body:FindFirstChild("BodyPanels")
    local s = p and p:FindFirstChild("Shoot")
    local cd = s and s:FindFirstChildOfClass("ClickDetector")
    if cd then fireclickdetector(cd) end
end

local function toggleSpam(type, enable)
    if spamConns[type] then
        spamConns[type]:Disconnect()
        spamConns[type] = nil
    end

    if not enable then return end

    spamConns[type] = game:GetService("RunService").Heartbeat:Connect(function()
        local v = workspace:FindFirstChild("Vehicles")
        if not v then return end

        for _, car in ipairs(v:GetChildren()) do
            if car:IsA("Model") then
                if type == "Own" and car.Name == player.Name .. "Car" then
                    explode(car)
                elseif type == "All" and car.Name ~= player.Name .. "Car" then
                    explode(car)
                end
            end
        end
    end)
end

Car:AddSwitch("Spam Explode Tank", function(state)
    toggleSpam("Own", state)
end)

Car:AddSwitch("Spam Explode Every Tank", function(state)
    toggleSpam("All", state)
end)

Car:AddLabel("Car Settings (Premium Only) ")
 
local runningCarRainbow = false
local RunService = game:GetService("RunService")
local carRemote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r")

-- Reaproveita a função de gerar cores suaves
local function getRandomColor()
	local r = math.random(50, 255) / 255
	local g = math.random(50, 255) / 255
	local b = math.random(50, 255) / 255
	return Color3.new(r, g, b)
end

local function lerpColor(a, b, t)
	local r = a.R + (b.R - a.R) * t
	local g = a.G + (b.G - a.G) * t
	local b = a.B + (b.B - a.B) * t
	return Color3.new(r, g, b)
end

local function smoothCarColorTransition()
	local currentColor = getRandomColor()
	local targetColor = getRandomColor()
	local duration = 2.5 -- pode ajustar conforme feeling

	while runningCarRainbow do
		local startTime = tick()
		while tick() - startTime < duration do
			if not runningCarRainbow then return end
			local elapsed = tick() - startTime
			local alpha = elapsed / duration
			local newColor = lerpColor(currentColor, targetColor, alpha)

			carRemote:FireServer("PickingCarColor", newColor)

			task.wait(0.1)
		end

		currentColor = targetColor
		targetColor = getRandomColor()
	end
end

Car:AddSwitch("Rainbow Car", function(state)
	runningCarRainbow = state
	if state then
		task.spawn(smoothCarColorTransition)
		
	else
		
	end
end)

Car:AddButton("Remove Wheel", function()
 args = {
    [1] = "BlowFrontLeft"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args))
wait()
 args = {
    [1] = "BlowFrontRight"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args))
wait()
 args = {
    [1] = "BlowRearRight"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args))
wait() 
args = {
    [1] = "BlowRearLeft"
}

game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args))
end)

Car:AddSwitch("Spam Fire", function(state)
    runningFire = state
    if state then
        spawn(function()
            while runningFire do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer("Fire")
                wait(2)
            end
        end)
        print("Loop Fire ON")
    else
        print("Loop Fire OFF")
    end
end)

local runningDuke1 = false
Car:AddSwitch("Spam Duke 1", function(state)
    runningDuke1 = state
    if state then
        spawn(function()
            while runningDuke1 do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer("Duke1")
                wait(1)
            end
        end)
        print("Loop Duke 1 ON")
    else
        print("Loop Duke 1 OFF")
    end
end)

-- Loop Duke 2
local runningDuke2 = false
Car:AddSwitch("Spam Duke 2", function(state)
    runningDuke2 = state
    if state then
        spawn(function()
            while runningDuke2 do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer("Duke")
                wait(1)
            end
        end)
        print("Loop Duke 2 ON")
    else
        print("Loop Duke 2 OFF")
    end
end)

Car:AddSwitch("Spam Turbo", function(state)
    runningSmoke = state
    if state then
        spawn(function()
            while runningSmoke do
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer("Smoke")
                wait(2)
            end
        end)
        print("Loop Fire ON")
    else
        print("Loop Fire OFF")
    end
end)

Car:AddLabel("Car Settings (Free) ")

local Player = game.Players.LocalPlayer
local Vehicles = workspace:FindFirstChild("Vehicles")

 function ApplyValueToCar(value)
    local car = Vehicles and Vehicles:FindFirstChild(Player.Name .. "Car")
    if car then
        local seat = car:FindFirstChild("Body") and car.Body:FindFirstChild("VehicleSeat")
        if seat then
            seat.TopSpeed.Value = value
            seat.Turbo.Value = value

             args = {
                [1] = "DriftingNumber",
                [2] = value
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r"):FireServer(unpack(args))

            print("Speed =  " .. value)
        else
            print("Error sit in car first")
        end
    else
        print("Spawn a car first !")
    end
end

Car:AddInput("Speed Box", "Enter id here...", function(input)
        local num = tonumber(input)
        if num then
            ApplyValueToCar(num)
        else
            print("...")
        end
    end)
    
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local remote = ReplicatedStorage:WaitForChild("RE"):WaitForChild("1Player1sCa1r")

-- Spam Horn
local hornActive = false
Car:AddSwitch("Spam Horn",  function(state)
    hornActive = state
    if state then
        spawn(function()
            while hornActive do
                remote:FireServer("Horn")
                wait(0.1)
            end
        end)
    end
end)

-- Spam Light
local lightActive = false
Car:AddSwitch("Spam Light", function(state)
    lightActive = state
    if state then
        spawn(function()
            while lightActive do
                remote:FireServer("Lights")
                wait(0.4)
            end
        end)
    end
end)

-- Spam Hazards
local hazardActive = false
Car:AddSwitch("Spam Hazards", function(state)
    hazardActive = state
    if state then
        spawn(function()
            while hazardActive do
                remote:FireServer("Hazards")
                wait(0.4)
            end
        end)
    end
end)

-- Spam Wheel
local wheelActive = false
Car:AddSwitch("Spam Wheel", function(state)
    wheelActive = state
    if state then
        spawn(function()
            while wheelActive do
                remote:FireServer("WheelNumber")
                wait(0.4)
            end
        end)
    end
end)

-- Spam Height
local heightActive = false
Car:AddSwitch("Spam Height", function(state)
    heightActive = state
    if state then
        spawn(function()
            while heightActive do
                remote:FireServer("VehicleHeight", 4)
                wait(1)
                remote:FireServer("VehicleHeight", 1)
                wait(1)
            end
        end)
    end
end)
    
Car:AddLabel("Bike Section")

local runningBikeRainbow = false
local bikeRemote = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Player1sCa1r")

-- Reutiliza função global se já existir
local function getRandomColor()
	local r = math.random(50, 255) / 255
	local g = math.random(50, 255) / 255
	local b = math.random(50, 255) / 255
	return Color3.new(r, g, b)
end

local function lerpColor(a, b, t)
	local r = a.R + (b.R - a.R) * t
	local g = a.G + (b.G - a.G) * t
	local b = a.B + (b.B - a.B) * t
	return Color3.new(r, g, b)
end

local function smoothBikeColorTransition()
	local currentColor = getRandomColor()
	local targetColor = getRandomColor()
	local duration = 2.5 -- ajuste de acordo com o efeito desejado

	while runningBikeRainbow do
		local startTime = tick()
		while tick() - startTime < duration do
			if not runningBikeRainbow then return end
			local elapsed = tick() - startTime
			local alpha = elapsed / duration
			local newColor = lerpColor(currentColor, targetColor, alpha)

			bikeRemote:FireServer("NoMotorColor", newColor)
			task.wait(0.1)
		end

		currentColor = targetColor
		targetColor = getRandomColor()
	end
end

Car:AddSwitch("Rainbow Bike", function(state)
	runningBikeRainbow = state
	if state then
		print("Rainbow Bike ativado.")
		task.spawn(smoothBikeColorTransition)
	else
		print("Rainbow Bike desativado.")
	end
end)

-- ==================
--  ⬇️Fun Buttons⬇️
-- ==================

Fun:AddLabel("Sign Section")
Fun:AddButton("Get Sign", function()
    args = {[1] = "PickingTools", [2] = "Sign"}		
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    game.Players.LocalPlayer.Backpack["Sign"].Parent = game.Players.LocalPlayer.Character
end)

sign1 = ""
sign2 = ""
sign3 = ""
sign4 = ""
sign5 = ""
repeatNames = false

Fun:AddInput("Sign 1", "Enter text here...", function(value)
    sign1 = value
end)
Fun:AddInput("Sign 2", "Enter text here...", function(value)
    sign2 = value
end)
Fun:AddInput("Sign 3", "Enter text here...", function(value)
    sign3 = value
end)
Fun:AddInput("Sign 4", "Enter text here...", function(value)
    sign4 = value
end)
Fun:AddInput("Sign 5", "Enter text here...", function(value)
    sign5 = value
end)

Fun:AddSwitch("Validate", function(value)
    repeatNames = value
    while repeatNames do
        local args1 = { "Sign", "SignWords", sign1 }
        game:GetService("Players").LocalPlayer.Character.Sign.ToolSound:FireServer(unpack(args1))
        wait(0.2)
        local args2 = { "Sign", "SignWords", sign2 }
        game:GetService("Players").LocalPlayer.Character.Sign.ToolSound:FireServer(unpack(args2))
        wait(0.2)
        local args3 = { "Sign", "SignWords", sign3 }
        game:GetService("Players").LocalPlayer.Character.Sign.ToolSound:FireServer(unpack(args3))
        wait(0.2)
        local args4 = { "Sign", "SignWords", sign4 }
        game:GetService("Players").LocalPlayer.Character.Sign.ToolSound:FireServer(unpack(args4))
        wait(0.2)
        local args5 = { "Sign", "SignWords", sign5 }
        game:GetService("Players").LocalPlayer.Character.Sign.ToolSound:FireServer(unpack(args5))
    end
end)

Fun:AddLabel("Box Section")
Fun:AddButton("Get Box", function()
    args = {[1] = "PickingTools", [2] = "Box"}		
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    game.Players.LocalPlayer.Backpack["Box"].Parent = game.Players.LocalPlayer.Character
end)

Box1 = ""
Box2 = ""
Box3 = ""
Box4 = ""
Box5 = ""
repeatNames1 = false

Fun:AddInput("Box 1", "Enter text here...", function(value)
    Box1 = value
end)
Fun:AddInput("Box 2", "Enter text here...", function(value)
    Box2 = value
end)
Fun:AddInput("Box 3", "Enter text here...", function(value)
    Box3 = value
end)
Fun:AddInput("Box 4", "Enter text here...", function(value)
    Box4 = value
end)
Fun:AddInput("Box 5", "Enter text here...", function(value)
    Box5 = value
end)

Fun:AddSwitch("Validate", function(value)
    repeatNames1 = value
    while repeatNames1 do
        local args1 = { "Box", "SignWords", Box1 }
        game:GetService("Players").LocalPlayer.Character.Box.ToolSound:FireServer(unpack(args1))
        wait(0.2)
        local args2 = { "Box", "SignWords", Box2 }
        game:GetService("Players").LocalPlayer.Character.Box.ToolSound:FireServer(unpack(args2))
        wait(0.2)
        local args3 = { "Box", "SignWords", Box3 }
        game:GetService("Players").LocalPlayer.Character.Box.ToolSound:FireServer(unpack(args3))
        wait(0.2)
        local args4 = { "Box", "SignWords", Box4 }
        game:GetService("Players").LocalPlayer.Character.Box.ToolSound:FireServer(unpack(args4))
        wait(0.2)
        local args5 = { "Box", "SignWords", Box5 }
        game:GetService("Players").LocalPlayer.Character.Box.ToolSound:FireServer(unpack(args5))
    end
end)

Fun:AddLabel("Roses Section")
Fun:AddButton("Get Roses", function()
    args = {"PickingTools", "Roses"}
    game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    local player = game.Players.LocalPlayer
    local rose = player.Backpack:FindFirstChild("Roses")
    if rose then
        rose.Parent = player.Character
    end
end)

local toggleActive = false
function RosesColor()
    task.spawn(function()
        while toggleActive do
            local char = game.Players.LocalPlayer.Character
            if not char then break end
            local rose = char:FindFirstChild("Roses")
            if not rose then break end
            local sound = rose:FindFirstChild("ToolSound")
            if not sound then break end

            local sounds = {
                "http://www.roblox.com/asset/?id=5210399458",
                "http://www.roblox.com/asset/?id=5210414520",
                "http://www.roblox.com/asset/?id=5216708760"
            }
            for _, id in ipairs(sounds) do
                if not toggleActive then break end
                sound:FireServer("Roses", id)
                task.wait(0.2)
            end
        end
    end)
end

Fun:AddSwitch("Rainbow Roses", function(value)
    toggleActive = value
    if toggleActive then
        RosesColor()
    end
end)

-- Universal Tool Counter
local function CountTool(toolName)
    local player = game.Players.LocalPlayer
    local total = 0
    for _, tool in ipairs(player.Backpack:GetChildren()) do
        if tool.Name == toolName then
            total += 1
        end
    end
    for _, tool in ipairs(player.Character:GetChildren()) do
        if tool.Name == toolName then
            total += 1
        end
    end
    return total
end

-- Universal Tool Duplicator
local function CreateDuplicator(toolName, labelName)
    Fun:AddLabel(labelName.." Section")

    local amount = 100

    Fun:AddInput("Amount", "", function(value)
        local number = tonumber(value)
        if number and number > 0 then
            amount = number
        else
            warn("Enter a valid number.")
        end
    end)

    Fun:AddButton("Dupe "..labelName, function()
        local player = game.Players.LocalPlayer
        local char = player.Character or player.CharacterAdded:Wait()
        local backpack = player:WaitForChild("Backpack")
        local root = char:WaitForChild("HumanoidRootPart")

        local giveTools = workspace:FindFirstChild("WorkspaceCom")
            and workspace.WorkspaceCom:FindFirstChild("001_GiveTools")
        local toolObject = giveTools and giveTools:FindFirstChild(toolName)

        if not (toolObject and toolObject:FindFirstChildOfClass("ClickDetector")) then
            warn("Tool not found in Workspace.")
            return
        end

        local detector = toolObject:FindFirstChildOfClass("ClickDetector")
        local oldPos = root.CFrame

        task.spawn(function()
            while CountTool(toolName) < amount do
                root.CFrame = toolObject.CFrame
                fireclickdetector(detector)
                task.wait(0.01)
            end
            root.CFrame = oldPos
            print("Duplication complete: "..toolName.." x"..CountTool(toolName))
        end)
    end)
end

CreateDuplicator("Taser", "Taser")
CreateDuplicator("FireX", "FireX")
CreateDuplicator("Assault", "Gun")
CreateDuplicator("Basketball", "Balls")

-- Balls Looper Section
local ThrowActive = false
local ThrowThread

Fun:AddSwitch("Balls Looper", function(state)
    ThrowActive = state

    if state then
        local player = game.Players.LocalPlayer
        local mouse = player:GetMouse()
        local char = player.Character or player.CharacterAdded:Wait()

        if not (player and mouse and char) then
            warn("Missing player, mouse or character")
            return
        end

        ThrowThread = task.spawn(function()
            while ThrowActive do
                for _, tool in ipairs(char:GetChildren()) do
                    if tool.Name == "Basketball" and tool:FindFirstChild("ClickEvent") then
                        tool.ClickEvent:FireServer(mouse.Hit.p)
                        task.wait(0.0003)
                    end
                end
                task.wait()
            end
        end)
    else
        if ThrowThread then
            task.cancel(ThrowThread)
            ThrowThread = nil
        end
    end
end)

Fun:AddLabel("Fling Section")
Fun:AddButton("Click Fling - By Shelby", function()
    loadstring(game:HttpGet('https://gist.githubusercontent.com/GistsPrivate/24ee609605cfaa52a28411cf8115535d/raw/5bac79672e8f1af9f98c92e1f0e232696f7f3858/gistfile1.txt'))()
end)

Fun:AddButton("FE DOORS", function()
    -- (Função igual ao original, apenas adaptada)
    local Players = game:GetService("Players")
    local Workspace = game:GetService("Workspace")
    local RunService = game:GetService("RunService")
    local UserInputService = game:GetService("UserInputService")

    local LocalPlayer = Players.LocalPlayer
    local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local HRP = Character:WaitForChild("HumanoidRootPart")

    for _, child in ipairs(HRP:GetChildren()) do
        if child.Name:match("^SHNMAX_AttachDoor_%d+$") then
            child:Destroy()
        end
    end

    local ControlledDoors = {}

    local function SetupPart(part, index)
        if not (part:IsA("BasePart") and not part.Anchored and part.Name:lower():find("door")) then return end
        if part:FindFirstChild("SHNMAX_Attached") then return end

        part.CanCollide = false
        for _, obj in ipairs(part:GetChildren()) do
            if obj:IsA("AlignPosition") or obj:IsA("Torque") or obj:IsA("Attachment") then
                obj:Destroy()
            end
        end

        Instance.new("BoolValue", part).Name = "SHNMAX_Attached"
        local doorAttachment = Instance.new("Attachment", part)
        doorAttachment.Name = "SHNMAX_LocalAttach"
        local hrpAttachment = Instance.new("Attachment", HRP)
        hrpAttachment.Name = "SHNMAX_AttachDoor_" .. index

        local align = Instance.new("AlignPosition", part)
        align.Attachment0 = doorAttachment
        align.Attachment1 = hrpAttachment
        align.MaxForce = 1e35
        align.MaxVelocity = math.huge
        align.Responsiveness = 99999

        local torque = Instance.new("Torque", part)
        torque.Attachment0 = doorAttachment
        torque.RelativeTo = Enum.ActuatorRelativeTo.World
        torque.Torque = Vector3.new(
            math.random(-1e19, 1e19),
            math.random(-1e19, 1e19),
            math.random(-1e25, 1e25)
        )

        table.insert(ControlledDoors, {
            Part = part,
            Align = align,
            Attach = doorAttachment,
            HRPAttach = hrpAttachment
        })
    end

    local index = 1
    for _, obj in ipairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") and obj.Name:lower():find("door") then
            SetupPart(obj, index)
            index += 1
        end
    end

    Workspace.DescendantAdded:Connect(function(obj)
        if obj:IsA("BasePart") and obj.Name:lower():find("door") then
            SetupPart(obj, #ControlledDoors + 1)
        end
    end)

    local function FlingPlayer(targetPlayer)
        local targetChar = targetPlayer.Character
        if not targetChar then return end

        local targetHRP = targetChar:FindFirstChild("HumanoidRootPart")
        if not targetHRP then return end

        local targetAttachment = targetHRP:FindFirstChild("SHNMAX_TargetAttachment")
        if not targetAttachment then
            targetAttachment = Instance.new("Attachment", targetHRP)
            targetAttachment.Name = "SHNMAX_TargetAttachment"
        end

        for _, door in ipairs(ControlledDoors) do
            if door.Align then
                door.Align.Attachment1 = targetAttachment
            end
        end

        local startTime = tick()
        local flingDone = false

        while tick() - startTime < 5 do
            if targetHRP.Velocity.Magnitude >= 40 then
                flingDone = true
                break
            end
            RunService.Heartbeat:Wait()
        end

        for _, door in ipairs(ControlledDoors) do
            if door.Align then
                door.Align.Attachment1 = door.HRPAttach
            end
        end

        print(string.format("[SHNMAX] Porta delivery para %s: %s", targetPlayer.Name, flingDone and "Flingado" or "Sem efeito"))
    end

    UserInputService.TouchTap:Connect(function(touchPositions, processed)
        if processed or not touchPositions[1] then return end
        local pos = touchPositions[1]
        local camera = Workspace.CurrentCamera
        local ray = camera:ScreenPointToRay(pos.X, pos.Y)
        local result = Workspace:Raycast(ray.Origin, ray.Direction * 1000)
        if result and result.Instance then
            local charModel = result.Instance:FindFirstAncestorOfClass("Model")
            local hitPlayer = Players:GetPlayerFromCharacter(charModel)
            if hitPlayer and hitPlayer ~= LocalPlayer then
                FlingPlayer(hitPlayer)
            end
        end
    end)
end)

Fun:AddLabel("Helicopter Section (Premium Only)")
Fun:AddLabel("Helicopter Spam Creates By :@Davi999")

Fun:AddSwitch("Helicopter Spam", function(state)
    getgenv().Toggle = state
    if state then
        local Players = game:GetService("Players")
        local Player = Players.LocalPlayer
        local Character = Player.Character
        local RootPart = Character.HumanoidRootPart
        local Humanoid = Character:FindFirstChildOfClass("Humanoid")
        local molestado1 = workspace.WorkspaceCom["001_HeliCloneButton"].Button
        task.spawn(function()
            while getgenv().Toggle do
                task.wait()
                RootPart.CFrame = molestado1.CFrame
                task.wait(0.1)
                fireclickdetector(molestado1.ClickDetector)
                task.wait(0.5)
                local molestado2 = workspace.WorkspaceCom["001_HeliStorage"]:FindFirstChild("PoliceStationHeli")
                if molestado2 then
                    repeat
                        RootPart.CFrame = molestado2.PrimaryPart.CFrame + Vector3.new(0, math.random(-1, 1), 0)
                        task.wait()
                    until Humanoid.Sit
                    task.wait(0.1)
                    Humanoid.Sit = false
                    task.wait()
                    repeat
                        RootPart.CFrame = molestado2.Passenger1.CFrame + Vector3.new(0, math.random(-1, 1), 0)
                        task.wait()
                    until Humanoid.Sit
                    task.wait(0.2)
                    RootPart.CFrame = CFrame.new(math.random(-40, 40), 4.549, math.random(-40, 40))
                    task.wait(0.2)
                    Humanoid.Sit = false
                    molestado2.Name = "PoliceStationHeliShnmaxhubAndCartolaHub"
                end
            end
        end)
    end
end)

Fun:AddSwitch("Canoe Spam", function(state)
    getgenv().Toggle = state
    if state then
        local Players = game:GetService("Players")
        local Player = Players.LocalPlayer
        local Character = Player.Character
        local RootPart = Character.HumanoidRootPart
        local Humanoid = Character:FindFirstChildOfClass("Humanoid")
        local molestado1 = workspace.WorkspaceCom["001_CanoeCloneButton"].Button
        task.spawn(function()
            while getgenv().Toggle do
                task.wait()
                RootPart.CFrame = molestado1.CFrame
                task.wait(0.1)
                fireclickdetector(molestado1.ClickDetector)
                task.wait(0.5)
                local molestado2 = workspace.WorkspaceCom["001_CanoeStorage"]:FindFirstChild("Canoe")
                if molestado2 and molestado2:FindFirstChild("VehicleSeat") then
                    repeat
                        RootPart.CFrame = molestado2.VehicleSeat.CFrame + Vector3.new(0, math.random(-1, 1), 0)
                        task.wait()
                    until Humanoid.Sit
                    task.wait(0.2)
                    RootPart.CFrame = CFrame.new(math.random(-40, 40), 4.549, math.random(-40, 40))
                    task.wait(0.2)
                    Humanoid.Sit = false
                    molestado2.Name = "CanoeShnmaxhubAndCartolaHub"
                end
            end
        end)
    end
end)

Fun:AddLabel("Helicopter Settings")
Fun:AddDropdown("Design List", {"Police", "Rescue", "Rich", "Military", "Agency", "Sheriff", "State Trooper"}, function(value)
    designHelico = value
end)

Fun:AddButton("Change Design", function()
    local meshIds = {
        Police = "PoliceMeshID",
        Rescue = "RescueMeshID",
        Rich = "TubeTVMeshID",
        Military = "MilitaryMeshID",
        Agency = "AgencyMeshID",
        Sheriff = "SheriffMeshID",
        ["State Trooper"] = "StateTrooperMeshID"
    }
    local meshId = meshIds[designHelico]
    if meshId then
        local args = {game.Players.LocalPlayer.Name, meshId}
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Max1y"):FireServer(unpack(args))
    end
end)

Fun:AddLabel("Horse Section")
Fun:AddButton("Tp All Horse", function()
    local players = game:GetService("Players")
    for _, player in pairs(players:GetPlayers()) do
        local args = {player}
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Hors1eRemot1e"):FireServer(unpack(args))
    end
end)

Fun:AddLabel("Modded Gun")
local function equipSniper()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local backpack = player.Backpack
    local sniperTool = character:FindFirstChild("Sniper") or backpack:FindFirstChild("Sniper")

    if not sniperTool then
        local args = {"PickingTools", "Sniper"}
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))
        task.wait(0.1)
        character.Humanoid:EquipTool(backpack:WaitForChild("Sniper"))
    elseif backpack:FindFirstChild("Sniper") then
        character.Humanoid:EquipTool(backpack["Sniper"])
    end
end

local function playSound(soundId)
    equipSniper()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local sniperHandle = character:FindFirstChild("Sniper") and character.Sniper:FindFirstChild("Handle")
    if sniperHandle then
        local sound = Instance.new("Sound")
        sound.SoundId = "rbxassetid://" .. tostring(soundId)
        sound.Volume = 0.1
        sound.Looped = false
        sound.Parent = player:WaitForChild("PlayerGui")
        sound:Play()
        sound.Ended:Connect(function()
            sound:Destroy()
        end)
        local args = {sniperHandle, soundId, 1}
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Gu1nSound1s"):FireServer(unpack(args))
    end
end

Fun:AddButton("Jumpscare Gun", function()
    playSound(85435253347146)
end)
Fun:AddButton("Sus Gun", function()
    playSound(6701126635)
end)
Fun:AddButton("amongus Gun", function()
    playSound(6651571134)
end)
Fun:AddButton("Troll Laugh", function()
    playSound(7816195044)
end)
Fun:AddButton("Scream Entidade666 Gun", function()
    playSound(9043346124)
end)
Fun:AddButton("Tubers93 Gun", function()
    playSound(103215672097028)
end)

Fun:AddLabel("Sounds Gun")
local soundIdFromTextBox = nil

-- Adaptação para sua GUI: AddInput, AddButton, AddLabel
local function equipSniper()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local backpack = player.Backpack
    local sniperTool = character:FindFirstChild("Sniper") or backpack:FindFirstChild("Sniper")

    if not sniperTool then
        local args = { "PickingTools", "Sniper" }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Too1l"):InvokeServer(unpack(args))
        task.wait(0.1)
        character.Humanoid:EquipTool(backpack:WaitForChild("Sniper"))
    elseif backpack:FindFirstChild("Sniper") then
        character.Humanoid:EquipTool(backpack["Sniper"])
    end
end

local function playSound(soundId)
    equipSniper()
    local player = game.Players.LocalPlayer
    local character = player.Character or player.CharacterAdded:Wait()
    local sniperHandle = character:FindFirstChild("Sniper") and character.Sniper:FindFirstChild("Handle")

    if sniperHandle then
        local sound = Instance.new("Sound")
        sound.SoundId = "rbxassetid://" .. tostring(soundId)
        sound.Volume = 0.1
        sound.Looped = false
        sound.Parent = player:WaitForChild("PlayerGui")
        sound:Play()
        sound.Ended:Connect(function()
            sound:Destroy()
        end)

        local args = { sniperHandle, tonumber(soundId), 1 }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Gu1nSound1s"):FireServer(unpack(args))
    end
end

local soundIdFromTextBox = nil

Fun:AddInput("Sounds Box", "Enter Id here...", function(text)
    local num = tonumber(text)
    if num then
        soundIdFromTextBox = num
    end
end)

Fun:AddButton("Validate", function()
    if soundIdFromTextBox then
        playSound(soundIdFromTextBox)
    end
end)

Fun:AddLabel("Chat Troll Section")

Fun:AddButton("Press! Troll Error FilterChatMessage.Reconnecting..", function()
    if game:GetService("TextChatService").ChatVersion == Enum.ChatVersion.TextChatService then
        game:GetService("TextChatService").TextChannels.RBXGeneral:SendAsync(
            "hi\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\r\SHNMAXSCRIPTS: HELLO EVERYONE ☑️"
        )
    else
        game:GetService("StarterGui"):SetCore("SendNotification", {
            Title = "Not Supported",
            Text = "This game has the legacy ROBLOX chat version. The script can only be used in the new version of the ROBLOX chat. Sorry :("
        })
    end
end)

-- ====================
--  ⬇️Itens Buttons⬇️
-- ====================

Itens:AddLabel("Section Dupe Tools")

-- Dropdown for Item Selection
local ItemDropdown = Itens:AddDropdown("Select Item to Duplicate", {
    "Couch", "Crystal", "Crystals", "DSLR Camera", "SoccerBall", "EggLauncher",
    "Cuffs", "FireHose", "AgencyBook", "KeyCardWhite", "DuffleBagDiamonds",
    "BankGateKey", "SwordGold", "OldKey", "PaintR
