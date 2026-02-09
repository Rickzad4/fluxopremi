--------------------------------------------------
-- SISTEMA DE KEY POR NOME (WHITELIST)
--------------------------------------------------

local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- 🔒 LISTA DE NOMES PERMITIDOS
local WHITELIST = {
    "Baron_e08",   -- coloque seu nome exatamente como no Roblox
    "minepikudao", -- você pode adicionar mais nomes
	"LaluluDoVine",
	"01pvpp",
}

local autorizado = false

for _, nome in ipairs(WHITELIST) do
    if player.Name == nome then
        autorizado = true
        break
    end
end

if not autorizado then
    warn("❌ Acesso negado para: " .. player.Name)
    task.wait(1)
    player:Kick("❌ Você não está autorizado a usar este script.")
    return -- impede o resto do script de rodar
end

print("✅ Acesso autorizado para:", player.Name)
--// FLUXO HUB COMPLETO COM SPEED, JUMP, HELICOPTERO AJUSTÁVEL, NOVO ANTI RAGDOLL, VISUALS, ANTI LAG E AUTO GRAB
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local Workspace = game:GetService("Workspace")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

if PlayerGui:FindFirstChild("FluxoHub") then
	PlayerGui.FluxoHub:Destroy()
end

--------------------------------------------------
-- UI BASE
--------------------------------------------------
local ScreenGui = Instance.new("ScreenGui", PlayerGui)
ScreenGui.Name = "FluxoHub"
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame", ScreenGui)
Main.Size = UDim2.new(0,600,0,360)
Main.Position = UDim2.new(0.5,-300,0.5,-180)
Main.BackgroundColor3 = Color3.fromRGB(10,10,10)
Instance.new("UICorner", Main).CornerRadius = UDim.new(0,22)

local Bg = Instance.new("ImageLabel", Main)
Bg.Size = UDim2.new(1,0,1,0)
Bg.BackgroundTransparency = 1
Bg.Image = "rbxassetid://1217190223"
Bg.ImageTransparency = 0.15

local Title = Instance.new("TextLabel", Main)
Title.Size = UDim2.new(1,0,0,50)
Title.BackgroundTransparency = 1
Title.Text = "F L U X O   H U B"
Title.Font = Enum.Font.GothamBold
Title.TextSize = 30
Title.TextColor3 = Color3.fromRGB(170,0,255)

local Sub = Instance.new("TextButton", Main)
Sub.Size = UDim2.new(1,0,0,20)
Sub.Position = UDim2.new(0,0,0,45)
Sub.BackgroundTransparency = 1
Sub.Text = "discord.gg/G7upZYU58Q"
Sub.Font = Enum.Font.Gotham
Sub.TextSize = 14
Sub.TextColor3 = Color3.fromRGB(170,0,255)
Sub.MouseButton1Click:Connect(function()
	setclipboard("discord.gg/G7upZYU58Q")
end)

local TitleLine = Instance.new("Frame", Main)
TitleLine.Size = UDim2.new(1,0,0,2)
TitleLine.Position = UDim2.new(0,0,0,70)
TitleLine.BackgroundColor3 = Color3.fromRGB(170,0,255)
TitleLine.BorderSizePixel = 0

--------------------------------------------------
-- MENU
--------------------------------------------------
local Menu = Instance.new("Frame", Main)
Menu.Size = UDim2.new(0,170,1,-70)
Menu.Position = UDim2.new(0,0,0,70)
Menu.BackgroundTransparency = 1

local VLine = Instance.new("Frame", Main)
VLine.Size = UDim2.new(0,2,1,-70)
VLine.Position = UDim2.new(0,170,0,70)
VLine.BackgroundColor3 = Color3.fromRGB(170,0,255)
VLine.BorderSizePixel = 0

local sections = {"COMBAT","MOVEMENT","VISUALS","MISC","CONFIG"}
for i,name in ipairs(sections) do
	local b = Instance.new("TextButton", Menu)
	b.Size = UDim2.new(1,0,0,45)
	b.Position = UDim2.new(0,0,0,(i-1)*50)
	b.BackgroundTransparency = 1
	b.Text = name
	b.Font = Enum.Font.Gotham
	b.TextSize = 18
	b.TextColor3 = Color3.fromRGB(170,0,255)
end

--------------------------------------------------
-- AUTO BAT
--------------------------------------------------
local AutoBatEnabled = false
local AutoBatFrame = Instance.new("Frame", Main)
AutoBatFrame.Size = UDim2.new(0,260,0,50)
AutoBatFrame.Position = UDim2.new(0,230,0,130)
AutoBatFrame.BackgroundTransparency = 1
AutoBatFrame.Visible = false

local AutoBatLabel = Instance.new("TextLabel", AutoBatFrame)
AutoBatLabel.Size = UDim2.new(0,150,1,0)
AutoBatLabel.BackgroundTransparency = 1
AutoBatLabel.Text = "AUTO BAT"
AutoBatLabel.Font = Enum.Font.Gotham
AutoBatLabel.TextSize = 18
AutoBatLabel.TextColor3 = Color3.fromRGB(170,0,255)
AutoBatLabel.TextXAlignment = Enum.TextXAlignment.Left

local AutoSwitch = Instance.new("TextButton", AutoBatFrame)
AutoSwitch.Size = UDim2.new(0,46,0,22)
AutoSwitch.Position = UDim2.new(1,-50,0.5,-11)
AutoSwitch.Text = ""
AutoSwitch.BackgroundColor3 = Color3.fromRGB(40,40,40)
AutoSwitch.AutoButtonColor = false
Instance.new("UICorner", AutoSwitch).CornerRadius = UDim.new(1,0)

local AutoBall = Instance.new("Frame", AutoSwitch)
AutoBall.Size = UDim2.new(0,18,0,18)
AutoBall.Position = UDim2.new(0,2,0.5,-9)
AutoBall.BackgroundColor3 = Color3.fromRGB(120,120,120)
Instance.new("UICorner", AutoBall).CornerRadius = UDim.new(1,0)

local function RefreshAuto()
	if AutoBatEnabled then
		AutoSwitch.BackgroundColor3 = Color3.fromRGB(170,0,255)
		AutoBall.Position = UDim2.new(1,-20,0.5,-9)
	else
		AutoSwitch.BackgroundColor3 = Color3.fromRGB(40,40,40)
		AutoBall.Position = UDim2.new(0,2,0.5,-9)
	end
end

AutoSwitch.MouseButton1Click:Connect(function()
	AutoBatEnabled = not AutoBatEnabled
	RefreshAuto()
end)

RefreshAuto()

task.spawn(function()
	while true do
		if AutoBatEnabled then
			local char = player.Character
			if char and char:FindFirstChild("HumanoidRootPart") then
				local tool = char:FindFirstChild("Bat") or char:FindFirstChild("Taco")
				if not tool then
					local bp = player.Backpack:FindFirstChild("Bat") or player.Backpack:FindFirstChild("Taco")
					if bp then bp.Parent = char tool = bp end
				end
				if tool then tool:Activate() end
			end
		end
		task.wait(0.05)
	end
end)

--------------------------------------------------
-- SETUP DO PERSONAGEM PARA SPEED/JUMP/HELICOPTERO
--------------------------------------------------
local Character, HRP, Humanoid
local function SetupCharacter(c)
	Character = c
	HRP = c:WaitForChild("HumanoidRootPart")
	Humanoid = c:WaitForChild("Humanoid")
end
if player.Character then SetupCharacter(player.Character) end
player.CharacterAdded:Connect(SetupCharacter)

--------------------------------------------------
-- MOVEMENT UI (SPEED, JUMP, HELICOPTERO AJUSTÁVEL, ANT RAGDOLL)
--------------------------------------------------
local MovementFrame = Instance.new("Frame", Main)
MovementFrame.Size = UDim2.new(0,320,0,280)
MovementFrame.Position = UDim2.new(0,230,0,100)
MovementFrame.BackgroundTransparency = 1
MovementFrame.Visible = false

-- VALORES INICIAIS CORRIGIDOS
local SpeedValue1 = 60
local SpeedValue2 = 30
local SpeedEnabled1 = false
local SpeedEnabled2 = false
local JumpEnabled = false
local HelicopteroEnabled = false
local HelicopteroSpeed = 30
local bodyAngularVelocity = nil

-- REFERÊNCIAS PARA OS CONTROLES DE SPEED
local speedControls = {}

-- FUNÇÃO PARA ATUALIZAR UI DO SPEED
local function UpdateSpeedUI()
	for _, control in pairs(speedControls) do
		local isEnabled = (control.index == 1 and SpeedEnabled1) or (control.index == 2 and SpeedEnabled2)
		control.switch.BackgroundColor3 = isEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		control.ball.Position = isEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end
end

-- FUNÇÃO PARA CRIAR SPEED
local function CreateSpeedControl(name, y, index)
	local f = Instance.new("Frame", MovementFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,y)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,100,1,0)
	label.BackgroundTransparency = 1
	label.Text = name
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local minus = Instance.new("TextButton", f)
	minus.Size = UDim2.new(0,25,1,0)
	minus.Position = UDim2.new(0,110,0,0)
	minus.BackgroundTransparency = 1
	minus.Text = "-"
	minus.Font = Enum.Font.GothamBold
	minus.TextSize = 20
	minus.TextColor3 = Color3.fromRGB(170,0,255)

	local valueLabel = Instance.new("TextLabel", f)
	valueLabel.Size = UDim2.new(0,40,1,0)
	valueLabel.Position = UDim2.new(0,140,0,0)
	valueLabel.BackgroundTransparency = 1
	valueLabel.Font = Enum.Font.Gotham
	valueLabel.TextSize = 16
	valueLabel.TextColor3 = Color3.fromRGB(170,0,255)

	local plus = Instance.new("TextButton", f)
	plus.Size = UDim2.new(0,25,1,0)
	plus.Position = UDim2.new(0,185,0,0)
	plus.BackgroundTransparency = 1
	plus.Text = "+"
	plus.Font = Enum.Font.GothamBold
	plus.TextSize = 20
	plus.TextColor3 = Color3.fromRGB(170,0,255)

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		local val = index==1 and SpeedValue1 or SpeedValue2
		valueLabel.Text = tostring(val)
		local on = index==1 and SpeedEnabled1 or SpeedEnabled2
		switch.BackgroundColor3 = on and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = on and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		if index==1 then
			SpeedEnabled1 = not SpeedEnabled1
			SpeedEnabled2 = false
		else
			SpeedEnabled2 = not SpeedEnabled2
			SpeedEnabled1 = false
		end
		UpdateSpeedUI()
	end)

	minus.MouseButton1Click:Connect(function()
		if index==1 then SpeedValue1 = math.max(5, SpeedValue1-5)
		else SpeedValue2 = math.max(5, SpeedValue2-5) end
		refresh()
	end)

	plus.MouseButton1Click:Connect(function()
		if index==1 then SpeedValue1 += 5
		else SpeedValue2 += 5 end
		refresh()
	end)

	-- Salvar referência para atualização
	table.insert(speedControls, {
		index = index,
		switch = switch,
		ball = ball,
		frame = f
	})

	refresh()
end

-- CRIAR SPEED 1 (Y=0) - AGORA COM VALOR 60
CreateSpeedControl("SPEED", 0, 1)

-- CRIAR SPEED 2 (Y=45, logo abaixo do Speed 1) - AGORA COM VALOR 30
CreateSpeedControl("SPEED 2", 45, 2)

-- JUMP (Y=90, abaixo do Speed 2)
local function CreateJumpToggle()
	local f = Instance.new("Frame", MovementFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,90)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "JUMP"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = JumpEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = JumpEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		JumpEnabled = not JumpEnabled
		refresh()
	end)

	refresh()
end
CreateJumpToggle()

-- HELICOPTERO AJUSTÁVEL (Y=135, abaixo do Jump)
local function CreateHelicopteroToggle()
	local f = Instance.new("Frame", MovementFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,135)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,100,1,0)
	label.BackgroundTransparency = 1
	label.Text = "HELICOPTERO"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local minus = Instance.new("TextButton", f)
	minus.Size = UDim2.new(0,25,1,0)
	minus.Position = UDim2.new(0,110,0,0)
	minus.BackgroundTransparency = 1
	minus.Text = "-"
	minus.Font = Enum.Font.GothamBold
	minus.TextSize = 20
	minus.TextColor3 = Color3.fromRGB(170,0,255)

	local valueLabel = Instance.new("TextLabel", f)
	valueLabel.Size = UDim2.new(0,40,1,0)
	valueLabel.Position = UDim2.new(0,140,0,0)
	valueLabel.BackgroundTransparency = 1
	valueLabel.Font = Enum.Font.Gotham
	valueLabel.TextSize = 16
	valueLabel.TextColor3 = Color3.fromRGB(170,0,255)

	local plus = Instance.new("TextButton", f)
	plus.Size = UDim2.new(0,25,1,0)
	plus.Position = UDim2.new(0,185,0,0)
	plus.BackgroundTransparency = 1
	plus.Text = "+"
	plus.Font = Enum.Font.GothamBold
	plus.TextSize = 20
	plus.TextColor3 = Color3.fromRGB(170,0,255)

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		valueLabel.Text = tostring(HelicopteroSpeed)
		switch.BackgroundColor3 = HelicopteroEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = HelicopteroEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		HelicopteroEnabled = not HelicopteroEnabled
		
		local character = player.Character
		if not character then 
			HelicopteroEnabled = false
			refresh()
			return 
		end
		
		local hrp = character:FindFirstChild("HumanoidRootPart")
		if not hrp then 
			HelicopteroEnabled = false
			refresh()
			return 
		end

		if HelicopteroEnabled then
			-- Ativa helicóptero
			if bodyAngularVelocity then bodyAngularVelocity:Destroy() end
			bodyAngularVelocity = Instance.new("BodyAngularVelocity")
			bodyAngularVelocity.MaxTorque = Vector3.new(0, math.huge, 0)
			bodyAngularVelocity.AngularVelocity = Vector3.new(0, HelicopteroSpeed, 0)
			bodyAngularVelocity.Parent = hrp
			print("🚁 Helicóptero ATIVADO - Velocidade: " .. HelicopteroSpeed)
		else
			-- Desativa helicóptero
			if bodyAngularVelocity then
				bodyAngularVelocity:Destroy()
				bodyAngularVelocity = nil
			end
			print("🚁 Helicóptero DESATIVADO")
		end
		refresh()
	end)

	minus.MouseButton1Click:Connect(function()
		HelicopteroSpeed = math.max(5, HelicopteroSpeed-5)
		refresh()
		
		-- Atualiza velocidade se estiver ativado
		if HelicopteroEnabled and bodyAngularVelocity then
			bodyAngularVelocity.AngularVelocity = Vector3.new(0, HelicopteroSpeed, 0)
			print("🚁 Velocidade do Helicóptero ajustada para: " .. HelicopteroSpeed)
		end
	end)

	plus.MouseButton1Click:Connect(function()
		HelicopteroSpeed += 5
		refresh()
		
		-- Atualiza velocidade se estiver ativado
		if HelicopteroEnabled and bodyAngularVelocity then
			bodyAngularVelocity.AngularVelocity = Vector3.new(0, HelicopteroSpeed, 0)
			print("🚁 Velocidade do Helicóptero ajustada para: " .. HelicopteroSpeed)
		end
	end)

	refresh()
end
CreateHelicopteroToggle()

--------------------------------------------------
-- NOVO ANTI RAGDOLL (SUBSTITUÍDO)
--------------------------------------------------
local AntiRagdollEnabled = false
local toggleAntiRagdollFunction = nil

task.spawn(function()
    local RunService = game:GetService("RunService")
    local LocalPlayer = player
    
    local isRunning = false
    local connections = {}
    local cachedCharData = {}
    
    local function cacheCharacterData()
        local char = LocalPlayer.Character
        if not char then return false end
        
        local hum = char:FindFirstChildOfClass("Humanoid")
        local root = char:FindFirstChild("HumanoidRootPart")
        
        if not hum or not root then return false end
        
        cachedCharData = {
            character = char,
            humanoid = hum,
            root = root,
        }
        
        return true
    end
    
    local function isRagdolled()
        if not cachedCharData.humanoid then return false end
        
        local hum = cachedCharData.humanoid
        local state = hum:GetState()
        
        local ragdollStates = {
            [Enum.HumanoidStateType.Physics] = true,
            [Enum.HumanoidStateType.Ragdoll] = true,
            [Enum.HumanoidStateType.FallingDown] = true
        }
        
        if ragdollStates[state] then
            return true
        end
        
        local endTime = LocalPlayer:GetAttribute("RagdollEndTime")
        if endTime then
            local now = workspace:GetServerTimeNow()
            if (endTime - now) > 0 then
                return true
            end
        end
        
        return false
    end
    
    local function preventRagdoll()
        if not cachedCharData.humanoid or not cachedCharData.root then return end
        
        local hum = cachedCharData.humanoid
        local root = cachedCharData.root
        
        pcall(function()
            local now = workspace:GetServerTimeNow()
            LocalPlayer:SetAttribute("RagdollEndTime", now - 1)
        end)
        
        if hum.Health > 0 then
            pcall(function()
                hum:ChangeState(Enum.HumanoidStateType.Running)
            end)
        end
        
        pcall(function()
            if root.Anchored then
                root.Anchored = false
            end
            root.AssemblyLinearVelocity = Vector3.zero
            root.AssemblyAngularVelocity = Vector3.zero
        end)
    end
    
    local function keepMotorJointsIntact()
        if not cachedCharData.character then return end
        
        for _, obj in ipairs(cachedCharData.character:GetDescendants()) do
            if obj:IsA("Motor6D") then
                if not obj.Enabled then
                    pcall(function()
                        obj.Enabled = true
                    end)
                end
            end
        end
    end
    
    local function setupCameraBinding()
        if not cachedCharData.humanoid then return end
        
        for _, conn in ipairs(connections) do
            if conn.Name == "Camera" then
                pcall(function() conn.Connection:Disconnect() end)
            end
        end
        
        local camConnection = RunService.Heartbeat:Connect(function()
            if not cachedCharData.humanoid then return end
            local cam = workspace.CurrentCamera
            if cam and cam.CameraSubject ~= cachedCharData.humanoid then
                cam.CameraSubject = cachedCharData.humanoid
            end
        end)
        
        table.insert(connections, {Name = "Camera", Connection = camConnection})
    end
    
    local function monitorDescendants()
        if not cachedCharData.character then return end
        
        for _, conn in ipairs(connections) do
            if conn.Name == "Descendant" then
                pcall(function() conn.Connection:Disconnect() end)
            end
        end
        
        local descendantConnection = cachedCharData.character.DescendantRemoving:Connect(function(descendant)
            if not _G.AntiRag then return end
            
            if descendant:IsA("Motor6D") then
                if not descendant.Parent then
                    local newMotor = descendant:Clone()
                    pcall(function()
                        newMotor.Parent = descendant.Parent
                    end)
                end
            end
        end)
        
        table.insert(connections, {Name = "Descendant", Connection = descendantConnection})
    end
    
    local function heartbeatLoop()
        while isRunning and cachedCharData.humanoid do
            task.wait()
            
            if not _G.AntiRag then
                task.wait(0.5)
                continue
            end
            
            if isRagdolled() then
                preventRagdoll()
                keepMotorJointsIntact()
            end
        end
    end
    
    local function onCharacterAdded(char)
        for _, conn in ipairs(connections) do
            pcall(function() conn.Connection:Disconnect() end)
        end
        connections = {}
        
        task.wait(0.5)
        
        if cacheCharacterData() then
            setupCameraBinding()
            monitorDescendants()
            isRunning = true
            task.spawn(heartbeatLoop)
        end
    end
    
    -- Inicializar Anti-Ragdoll baseado na configuração salva
    _G.AntiRag = AntiRagdollEnabled
    
    if cacheCharacterData() then
        setupCameraBinding()
        monitorDescendants()
        isRunning = true
        task.spawn(heartbeatLoop)
    end
    
    LocalPlayer.CharacterAdded:Connect(onCharacterAdded)
    
    -- Função para alternar Anti-Ragdoll
    toggleAntiRagdollFunction = function(state)
        _G.AntiRag = state
        AntiRagdollEnabled = state
        
        if state then
            print("✅ Anti-Ragdoll ATIVADO")
        else
            print("❌ Anti-Ragdoll DESATIVADO")
        end
    end
    
    -- Inicializar com estado padrão
    toggleAntiRagdollFunction(AntiRagdollEnabled)
end)

-- ANT RAGDOLL TOGGLE NA UI (Y=180, abaixo do Helicoptero)
local function CreateAntiRagdollToggle()
	local f = Instance.new("Frame", MovementFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,180)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "ANT RAGDOLL"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = AntiRagdollEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = AntiRagdollEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		AntiRagdollEnabled = not AntiRagdollEnabled
		if toggleAntiRagdollFunction then
			toggleAntiRagdollFunction(AntiRagdollEnabled)
		end
		refresh()
	end)

	refresh()
end
CreateAntiRagdollToggle()

-- TOGGLE PELO TECLADO (H = ligar/desligar)
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.H then
        AntiRagdollEnabled = not AntiRagdollEnabled
        if toggleAntiRagdollFunction then
            toggleAntiRagdollFunction(AntiRagdollEnabled)
        end
        
        -- Atualizar UI
        for _, child in pairs(MovementFrame:GetChildren()) do
            if child:IsA("Frame") then
                local label = child:FindFirstChildOfClass("TextLabel")
                if label and label.Text == "ANT RAGDOLL" then
                    local switch = child:FindFirstChildOfClass("TextButton")
                    if switch then
                        switch.BackgroundColor3 = AntiRagdollEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
                        local ball = switch:FindFirstChildOfClass("Frame")
                        if ball then
                            ball.Position = AntiRagdollEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
                        end
                    end
                end
            end
        end
    end
end)

--------------------------------------------------
-- VISUALS UI (XRAY, ESP ALL, TIMER ESP, ANT LAG)
--------------------------------------------------
local VisualsFrame = Instance.new("Frame", Main)
VisualsFrame.Size = UDim2.new(0,320,0,280)
VisualsFrame.Position = UDim2.new(0,230,0,100)
VisualsFrame.BackgroundTransparency = 1
VisualsFrame.Visible = false

-- VARIÁVEIS PARA VISUALS
local XRayEnabled = false
local ESPAllEnabled = false
local TimerESPEnabled = false

--------------------------------------------------
-- XRAY SISTEMA (SEU SCRIPT)
--------------------------------------------------
local XRayConnection = nil
local XRAY_BASE_NAME = "Base"
local XRAY_TRANSPARENCY = 0.7

local function AplicarTransparenciaXRay()
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("BasePart") and v.Name:lower():find(XRAY_BASE_NAME:lower()) then
            v.LocalTransparencyModifier = XRAY_TRANSPARENCY
        end
    end
end

local function RemoverTransparenciaXRay()
    for _, v in pairs(Workspace:GetDescendants()) do
        if v:IsA("BasePart") and v.Name:lower():find(XRAY_BASE_NAME:lower()) then
            v.LocalTransparencyModifier = 0
        end
    end
end

local function AtivarXRay()
    if XRayEnabled then return end
    XRayEnabled = true
    AplicarTransparenciaXRay()

    -- Aplica transparência a novos objetos que aparecerem
    XRayConnection = Workspace.DescendantAdded:Connect(function(obj)
        task.wait(0.1)
        if XRayEnabled and obj:IsA("BasePart") and obj.Name:lower():find(XRAY_BASE_NAME:lower()) then
            obj.LocalTransparencyModifier = XRAY_TRANSPARENCY
        end
    end)

    print("👁️ X-Ray ATIVADO")
end

local function DesativarXRay()
    if not XRayEnabled then return end
    XRayEnabled = false
    RemoverTransparenciaXRay()

    if XRayConnection then
        XRayConnection:Disconnect()
        XRayConnection = nil
    end

    print("👁️ X-Ray DESATIVADO")
end

-- XRAY TOGGLE NA UI
local function CreateXrayToggle()
	local f = Instance.new("Frame", VisualsFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,0)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "XRAY"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = XRayEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = XRayEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		if XRayEnabled then
			DesativarXRay()
		else
			AtivarXRay()
		end
		refresh()
	end)

	refresh()
end
CreateXrayToggle()

--------------------------------------------------
-- ESP ALL (DO HYPER TOOLS)
--------------------------------------------------
local espHighlights = {}
local espNames = {}

local function CreateESPAllToggle()
	local f = Instance.new("Frame", VisualsFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,45)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "ESP ALL"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = ESPAllEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = ESPAllEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	-- Cria highlight e nome para um personagem
	local function createESP(char)
		if not char or not char:FindFirstChild("HumanoidRootPart") then return end
		
		-- Highlight
		local highlight = Instance.new("Highlight")
		highlight.Adornee = char
		highlight.FillColor = Color3.fromRGB(170,0,255)
		highlight.OutlineColor = Color3.fromRGB(170,0,255)
		highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
		highlight.Parent = char
		table.insert(espHighlights, highlight)
		
		-- Nome acima da cabeça
		local billboard = Instance.new("BillboardGui")
		billboard.Size = UDim2.new(0, 100, 0, 50)
		billboard.Adornee = char:FindFirstChild("Head")
		billboard.AlwaysOnTop = true
		billboard.Parent = char

		local nameLabel = Instance.new("TextLabel")
		nameLabel.BackgroundTransparency = 1
		nameLabel.Size = UDim2.new(1,0,1,0)
		nameLabel.Font = Enum.Font.Gotham
		nameLabel.Text = char.Name
		nameLabel.TextColor3 = Color3.fromRGB(170,0,255)
		nameLabel.TextScaled = true
		nameLabel.TextStrokeTransparency = 0.5
		nameLabel.Parent = billboard

		table.insert(espNames, {billboard, nameLabel})
	end

	-- Ativa ESP para todos os jogadores
	local function toggleESPAll()
		ESPAllEnabled = not ESPAllEnabled
		if ESPAllEnabled then
			refresh()
			for _, plr in ipairs(Players:GetPlayers()) do
				if plr ~= player and plr.Character then
					createESP(plr.Character)
				end
			end
			-- Atualiza novos personagens que entrarem
			Players.PlayerAdded:Connect(function(plr)
				plr.CharacterAdded:Connect(function(char)
					if ESPAllEnabled then
						createESP(char)
					end
				end)
			end)
		else
			refresh()
			-- Remove tudo
			for _, hl in ipairs(espHighlights) do
				hl:Destroy()
			end
			espHighlights = {}
			for _, data in ipairs(espNames) do
				if data[1] then data[1]:Destroy() end
			end
			espNames = {}
		end
	end

	switch.MouseButton1Click:Connect(toggleESPAll)

	refresh()
end
CreateESPAllToggle()

--------------------------------------------------
-- TIMER ESP (MOSTRA O TIMER DA BASE DE CADA PESSOA)
--------------------------------------------------
local function CreateTimerESPToggle()
	local f = Instance.new("Frame", VisualsFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,90)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "TIMER ESP"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = TimerESPEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = TimerESPEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	-- Dicionário para armazenar os timers das bases
	local playerTimers = {}
	local timerEspGuis = {}

	local function findPlayerBase(player)
		-- Procurar pela base do jogador no workspace
		for _, obj in ipairs(Workspace:GetChildren()) do
			-- Verificar se o objeto tem o nome do jogador ou é uma base
			if obj.Name:lower():find(player.Name:lower()) or 
			   obj.Name:lower():find("base") or
			   (obj:FindFirstChild("Owner") and obj.Owner.Value == player) then
				-- Procurar por um timer dentro da base
				local timer = findTimerInModel(obj)
				if timer then
					return obj, timer
				end
			end
		end
		return nil, nil
	end

	local function findTimerInModel(model)
		-- Procurar por um timer dentro do modelo
		for _, child in ipairs(model:GetDescendants()) do
			if child.Name:lower():find("timer") or 
			   child.Name:lower():find("bomb") or
			   child:IsA("TextLabel") and child.Text:lower():find("timer") then
				return child
			end
		end
		return nil
	end

	local function createTimerESPForPlayer(player, base, timer)
		if not base or not timer then return end
		
		-- Criar um BillboardGui para mostrar o timer
		local timerEsp = Instance.new("BillboardGui")
		timerEsp.Name = "TimerESP_" .. player.Name
		timerEsp.AlwaysOnTop = true
		timerEsp.Size = UDim2.new(0, 300, 0, 80)
		timerEsp.StudsOffset = Vector3.new(0, 8, 0)
		timerEsp.MaxDistance = 500
		
		local timerLabel = Instance.new("TextLabel", timerEsp)
		timerLabel.BackgroundTransparency = 1
		timerLabel.Size = UDim2.new(1, 0, 0.5, 0)
		timerLabel.Text = player.Name .. "'s Base"
		timerLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
		timerLabel.TextSize = 16
		timerLabel.Font = Enum.Font.GothamBold
		
		local timeLabel = Instance.new("TextLabel", timerEsp)
		timeLabel.BackgroundTransparency = 1
		timeLabel.Size = UDim2.new(1, 0, 0.5, 0)
		timeLabel.Position = UDim2.new(0, 0, 0.5, 0)
		timeLabel.Text = "⏰ TIMER"
		timeLabel.TextColor3 = Color3.fromRGB(255, 255, 0)
		timeLabel.TextSize = 18
		timeLabel.Font = Enum.Font.GothamBold
		
		-- Conectar ao timer para atualizar o texto
		if timer:IsA("TextLabel") or timer:IsA("TextButton") or timer:IsA("TextBox") then
			local connection
			connection = RunService.Heartbeat:Connect(function()
				if TimerESPEnabled and timerEsp and timerEsp.Parent then
					timeLabel.Text = "⏰ " .. tostring(timer.Text)
				else
					if connection then
						connection:Disconnect()
					end
				end
			end)
		end
		
		-- Encontrar uma parte para anexar o ESP
		local primaryPart = base.PrimaryPart or base:FindFirstChildWhichIsA("BasePart")
		if primaryPart then
			timerEsp.Adornee = primaryPart
			timerEsp.Parent = primaryPart
		else
			-- Se não encontrar parte, criar uma part temporária
			local tempPart = Instance.new("Part")
			tempPart.Name = "TempESPAnchor"
			tempPart.Size = Vector3.new(1, 1, 1)
			tempPart.Transparency = 1
			tempPart.CanCollide = false
			tempPart.Anchored = true
			tempPart.Position = base:GetPivot().Position + Vector3.new(0, 5, 0)
			tempPart.Parent = base
			
			timerEsp.Adornee = tempPart
			timerEsp.Parent = tempPart
		end
		
		timerEspGuis[player] = timerEsp
		playerTimers[player] = {base = base, timer = timer}
	end

	local function removeAllTimerESP()
		for _, timerEsp in pairs(timerEspGuis) do
			if timerEsp then 
				timerEsp:Destroy()
			end
		end
		timerEspGuis = {}
		playerTimers = {}
	end

	local function updateTimerESP()
		if TimerESPEnabled then
			removeAllTimerESP()
			
			-- Para cada jogador, encontrar sua base e timer
			for _, otherPlayer in ipairs(Players:GetPlayers()) do
				if otherPlayer ~= player then
					local base, timer = findPlayerBase(otherPlayer)
					if base and timer then
						createTimerESPForPlayer(otherPlayer, base, timer)
					end
				end
			end
			
			-- Monitorar novos jogadores
			Players.PlayerAdded:Connect(function(newPlayer)
				if TimerESPEnabled and newPlayer ~= player then
					task.wait(2) -- Esperar a base carregar
					local base, timer = findPlayerBase(newPlayer)
					if base and timer then
						createTimerESPForPlayer(newPlayer, base, timer)
					end
				end
			end)
			
			-- Monitorar remoção de jogadores
			Players.PlayerRemoving:Connect(function(removedPlayer)
				if timerEspGuis[removedPlayer] then
					timerEspGuis[removedPlayer]:Destroy()
					timerEspGuis[removedPlayer] = nil
					playerTimers[removedPlayer] = nil
				end
			end)
			
			-- Monitorar novas bases
			Workspace.DescendantAdded:Connect(function(descendant)
				if TimerESPEnabled and descendant:IsA("Model") then
					-- Verificar se é uma base de algum jogador
					for _, otherPlayer in ipairs(Players:GetPlayers()) do
						if otherPlayer ~= player and not playerTimers[otherPlayer] then
							if descendant.Name:lower():find(otherPlayer.Name:lower()) or 
							   descendant.Name:lower():find("base") then
								local timer = findTimerInModel(descendant)
								if timer then
									createTimerESPForPlayer(otherPlayer, descendant, timer)
								end
							end
						end
					end
				end
			end)
		else
			removeAllTimerESP()
		end
	end

	switch.MouseButton1Click:Connect(function()
		TimerESPEnabled = not TimerESPEnabled
		updateTimerESP()
		refresh()
		print(TimerESPEnabled and "⏰ Timer ESP ATIVADO" or "⏰ Timer ESP DESATIVADO")
	end)

	refresh()
end
CreateTimerESPToggle()

--------------------------------------------------
-- ANTI LAG COMPLETO (SEU SCRIPT) - COM RESTAURAÇÃO PERFEITA
--------------------------------------------------
local AntiLagEnabled = false

-- TABELA PARA SALVAR CONFIGURAÇÕES ORIGINAIS
local ConfiguracoesOriginais = {
    QualityLevel = nil,
    GlobalShadows = nil,
    FogEnd = nil,
    EnvironmentDiffuseScale = nil,
    EnvironmentSpecularScale = nil,
    PartMaterials = {}, -- Para salvar materiais originais das partes
    PartCastShadow = {}, -- Para salvar sombras originais
    ParticleEnabled = {}, -- Para salvar estado das partículas
    TrailEnabled = {}, -- Para salvar estado das trilhas
    SmokeEnabled = {}, -- Para salvar estado das fumaças
    FireEnabled = {} -- Para salvar estado dos fogos
}

--// FUNÇÃO PARA SALVAR CONFIGURAÇÕES ORIGINAIS
local function SalvarConfiguracoesOriginais()
    -- Salvar configurações gráficas
    ConfiguracoesOriginais.QualityLevel = settings().Rendering.QualityLevel
    ConfiguracoesOriginais.GlobalShadows = Lighting.GlobalShadows
    ConfiguracoesOriginais.FogEnd = Lighting.FogEnd
    ConfiguracoesOriginais.EnvironmentDiffuseScale = Lighting.EnvironmentDiffuseScale
    ConfiguracoesOriginais.EnvironmentSpecularScale = Lighting.EnvironmentSpecularScale
    
    -- Limpar tabelas antigas
    ConfiguracoesOriginais.PartMaterials = {}
    ConfiguracoesOriginais.PartCastShadow = {}
    ConfiguracoesOriginais.ParticleEnabled = {}
    ConfiguracoesOriginais.TrailEnabled = {}
    ConfiguracoesOriginais.SmokeEnabled = {}
    ConfiguracoesOriginais.FireEnabled = {}
    
    -- Salvar estado de todos os objetos do workspace
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            ConfiguracoesOriginais.PartMaterials[obj] = obj.Material
            ConfiguracoesOriginais.PartCastShadow[obj] = obj.CastShadow
        elseif obj:IsA("ParticleEmitter") then
            ConfiguracoesOriginais.ParticleEnabled[obj] = obj.Enabled
        elseif obj:IsA("Trail") then
            ConfiguracoesOriginais.TrailEnabled[obj] = obj.Enabled
        elseif obj:IsA("Smoke") then
            ConfiguracoesOriginais.SmokeEnabled[obj] = obj.Enabled
        elseif obj:IsA("Fire") then
            ConfiguracoesOriginais.FireEnabled[obj] = obj.Enabled
        end
    end
    
    print("✅ Configurações originais salvas")
end

--// FUNÇÃO PARA ATIVAR ANTI LAG
local function AtivarAntiLag()
    if AntiLagEnabled then return end
    
    -- Primeiro salva as configurações atuais
    SalvarConfiguracoesOriginais()
    
    AntiLagEnabled = true
    print("🧊 Anti Lag ATIVADO")

    -- Configurações gráficas mínimas
    settings().Rendering.QualityLevel = Enum.QualityLevel.Level01
    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9
    Lighting.EnvironmentDiffuseScale = 0
    Lighting.EnvironmentSpecularScale = 0

    -- Ajuste de objetos do workspace
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            obj.Material = Enum.Material.Plastic
            obj.CastShadow = false
        elseif obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Smoke") or obj:IsA("Fire") then
            obj.Enabled = false
        end
    end

    -- Notificação opcional
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Anti Lag",
            Text = "Anti Lag ATIVADO",
            Duration = 3
        })
    end)
end

--// FUNÇÃO PARA DESATIVAR ANTI LAG E RESTAURAR TUDO
local function DesativarAntiLag()
    if not AntiLagEnabled then return end
    
    print("🧊 Anti Lag DESATIVADO - Restaurando configurações...")

    -- Restaura configurações gráficas originais
    if ConfiguracoesOriginais.QualityLevel then
        settings().Rendering.QualityLevel = ConfiguracoesOriginais.QualityLevel
    else
        settings().Rendering.QualityLevel = Enum.QualityLevel.Level21
    end
    
    if ConfiguracoesOriginais.GlobalShadows ~= nil then
        Lighting.GlobalShadows = ConfiguracoesOriginais.GlobalShadows
    else
        Lighting.GlobalShadows = true
    end
    
    if ConfiguracoesOriginais.FogEnd then
        Lighting.FogEnd = ConfiguracoesOriginais.FogEnd
    else
        Lighting.FogEnd = 100000
    end
    
    if ConfiguracoesOriginais.EnvironmentDiffuseScale then
        Lighting.EnvironmentDiffuseScale = ConfiguracoesOriginais.EnvironmentDiffuseScale
    else
        Lighting.EnvironmentDiffuseScale = 1
    end
    
    if ConfiguracoesOriginais.EnvironmentSpecularScale then
        Lighting.EnvironmentSpecularScale = ConfiguracoesOriginais.EnvironmentSpecularScale
    else
        Lighting.EnvironmentSpecularScale = 1
    end

    -- Restaura objetos do workspace
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            -- Restaura material original
            if ConfiguracoesOriginais.PartMaterials[obj] then
                obj.Material = ConfiguracoesOriginais.PartMaterials[obj]
            else
                obj.Material = Enum.Material.Plastic -- Fallback
            end
            
            -- Restaura sombra original
            if ConfiguracoesOriginais.PartCastShadow[obj] ~= nil then
                obj.CastShadow = ConfiguracoesOriginais.PartCastShadow[obj]
            else
                obj.CastShadow = true -- Fallback
            end
        elseif obj:IsA("ParticleEmitter") then
            if ConfiguracoesOriginais.ParticleEnabled[obj] ~= nil then
                obj.Enabled = ConfiguracoesOriginais.ParticleEnabled[obj]
            else
                obj.Enabled = true -- Fallback
            end
        elseif obj:IsA("Trail") then
            if ConfiguracoesOriginais.TrailEnabled[obj] ~= nil then
                obj.Enabled = ConfiguracoesOriginais.TrailEnabled[obj]
            else
                obj.Enabled = true -- Fallback
            end
        elseif obj:IsA("Smoke") then
            if ConfiguracoesOriginais.SmokeEnabled[obj] ~= nil then
                obj.Enabled = ConfiguracoesOriginais.SmokeEnabled[obj]
            else
                obj.Enabled = true -- Fallback
            end
        elseif obj:IsA("Fire") then
            if ConfiguracoesOriginais.FireEnabled[obj] ~= nil then
                obj.Enabled = ConfiguracoesOriginais.FireEnabled[obj]
            else
                obj.Enabled = true -- Fallback
            end
        end
    end

    AntiLagEnabled = false
    
    -- Notificação
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "Anti Lag",
            Text = "Configurações restauradas",
            Duration = 3
        })
    end)
    
    print("✅ Configurações completamente restauradas")
end

-- ANTI LAG TOGGLE NA UI
local function CreateAntiLagToggle()
	local f = Instance.new("Frame", VisualsFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,135)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "ANTI LAG"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = AntiLagEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = AntiLagEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		if AntiLagEnabled then
			DesativarAntiLag()
		else
			AtivarAntiLag()
		end
		refresh()
	end)

	refresh()
end
CreateAntiLagToggle()

--------------------------------------------------
-- MISC FRAME (AUTO GRAB COM FUNÇÃO COMPLETA)
--------------------------------------------------
local MiscFrame = Instance.new("Frame", Main)
MiscFrame.Size = UDim2.new(0,320,0,280)
MiscFrame.Position = UDim2.new(0,230,0,100)
MiscFrame.BackgroundTransparency = 1
MiscFrame.Visible = false

-- AUTO GRAB VARIÁVEL E SISTEMA
local AutoGrabEnabled = false
local AutoGrabSystem = nil
local ProximityPromptService = game:GetService("ProximityPromptService")

--// FUNÇÃO PARA INICIAR O AUTO GRAB COMPLETO
local function StartAutoGrab()
    if AutoGrabSystem then return end
    
    print("🔥 AUTO GRAB ATIVADO")
    
    -- Sistema de Auto Grab
    AutoGrabSystem = {
        Running = true,
        Connections = {},
        TARGET_TOOL = "Flying Carpet" -- mude se quiser
    }
    
    --// EQUIPAR FERRAMENTA
    local function EquipTool()
        local char = player.Character
        if not char then return end

        if char:FindFirstChild(AutoGrabSystem.TARGET_TOOL) then return end

        local backpack = player:FindFirstChild("Backpack")
        if backpack then
            local tool = backpack:FindFirstChild(AutoGrabSystem.TARGET_TOOL)
            if tool then
                tool.Parent = char
            end
        end
    end

    --// PROCESSAR PROMPT
    local function HandlePrompt(prompt)
        if not AutoGrabSystem or not AutoGrabSystem.Running then return end

        local text = (prompt.ActionText .. prompt.ObjectText):lower()
        if not (text:find("grab") or text:find("take") or text:find("steal")) then
            return
        end

        task.spawn(function()
            local holding = false

            while prompt and prompt.Parent and AutoGrabSystem and AutoGrabSystem.Running do
                task.wait(0.1)

                local pos
                if prompt.Parent:IsA("Attachment") then
                    pos = prompt.Parent.WorldPosition
                elseif prompt.Parent:IsA("BasePart") then
                    pos = prompt.Parent.Position
                elseif prompt.Parent:IsA("Model") then
                    pos = prompt.Parent:GetPivot().Position
                end
                if not pos then continue end

                local dist = player:DistanceFromCharacter(pos)
                local maxDist = prompt.MaxActivationDistance or 10

                if dist <= maxDist then
                    if not holding then
                        EquipTool()
                        task.wait(0.05)
                        pcall(function()
                            prompt:InputHoldBegin()
                        end)
                        holding = true
                    end
                else
                    if holding then
                        pcall(function()
                            prompt:InputHoldEnd()
                        end)
                        holding = false
                    end
                end
            end
        end)
    end

    --// PROMPTS JÁ EXISTENTES
    for _, obj in pairs(Workspace:GetDescendants()) do
        if obj:IsA("ProximityPrompt") then
            HandlePrompt(obj)
        end
    end

    --// NOVOS PROMPTS
    table.insert(AutoGrabSystem.Connections, Workspace.DescendantAdded:Connect(function(obj)
        if obj:IsA("ProximityPrompt") then
            HandlePrompt(obj)
        end
    end))

    table.insert(AutoGrabSystem.Connections, ProximityPromptService.PromptShown:Connect(function(prompt)
        HandlePrompt(prompt)
    end))
end

--// FUNÇÃO PARA PARAR O AUTO GRAB
local function StopAutoGrab()
    if not AutoGrabSystem then return end
    
    print("🔥 AUTO GRAB DESATIVADO")
    
    AutoGrabSystem.Running = false
    
    -- Desconectar todas as conexões
    for _, connection in ipairs(AutoGrabSystem.Connections) do
        pcall(function()
            connection:Disconnect()
        end)
    end
    
    AutoGrabSystem.Connections = {}
    AutoGrabSystem = nil
end

-- AUTO GRAB TOGGLE (POSIÇÃO 0,0)
local function CreateAutoGrabToggle()
	local f = Instance.new("Frame", MiscFrame)
	f.Size = UDim2.new(0,300,0,45)
	f.Position = UDim2.new(0,0,0,0)
	f.BackgroundTransparency = 1

	local label = Instance.new("TextLabel", f)
	label.Size = UDim2.new(0,150,1,0)
	label.BackgroundTransparency = 1
	label.Text = "AUTO GRAB"
	label.Font = Enum.Font.Gotham
	label.TextSize = 18
	label.TextColor3 = Color3.fromRGB(170,0,255)
	label.TextXAlignment = Enum.TextXAlignment.Left

	local switch = Instance.new("TextButton", f)
	switch.Size = UDim2.new(0,46,0,22)
	switch.Position = UDim2.new(1,-50,0.5,-11)
	switch.Text = ""
	switch.BackgroundColor3 = Color3.fromRGB(40,40,40)
	switch.AutoButtonColor = false
	Instance.new("UICorner", switch).CornerRadius = UDim.new(1,0)

	local ball = Instance.new("Frame", switch)
	ball.Size = UDim2.new(0,18,0,18)
	ball.Position = UDim2.new(0,2,0.5,-9)
	ball.BackgroundColor3 = Color3.fromRGB(120,120,120)
	Instance.new("UICorner", ball).CornerRadius = UDim.new(1,0)

	local function refresh()
		switch.BackgroundColor3 = AutoGrabEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
		ball.Position = AutoGrabEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
	end

	switch.MouseButton1Click:Connect(function()
		AutoGrabEnabled = not AutoGrabEnabled
		
		if AutoGrabEnabled then
			-- Inicia o sistema completo de Auto Grab
			StartAutoGrab()
		else
			-- Para o sistema de Auto Grab
			StopAutoGrab()
		end
		
		refresh()
	end)

	refresh()
end
CreateAutoGrabToggle()

-- TOGGLE PELO TECLADO (G = ligar/desligar Auto Grab)
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.G then
        AutoGrabEnabled = not AutoGrabEnabled
        
        if AutoGrabEnabled then
            -- Inicia o sistema completo de Auto Grab
            StartAutoGrab()
        else
            -- Para o sistema de Auto Grab
            StopAutoGrab()
        end
        
        -- Atualizar UI
        for _, child in pairs(MiscFrame:GetChildren()) do
            if child:IsA("Frame") then
                local label = child:FindFirstChildOfClass("TextLabel")
                if label and label.Text == "AUTO GRAB" then
                    local switch = child:FindFirstChildOfClass("TextButton")
                    if switch then
                        switch.BackgroundColor3 = AutoGrabEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
                        local ball = switch:FindFirstChildOfClass("Frame")
                        if ball then
                            ball.Position = AutoGrabEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
                        end
                    end
                end
            end
        end
    end
end)

-- TOGGLE PELO TECLADO (L = ligar/desligar Anti Lag)
UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.L then
        if AntiLagEnabled then
            DesativarAntiLag()
        else
            AtivarAntiLag()
        end
        
        -- Atualizar UI
        for _, child in pairs(VisualsFrame:GetChildren()) do
            if child:IsA("Frame") then
                local label = child:FindFirstChildOfClass("TextLabel")
                if label and label.Text == "ANTI LAG" then
                    local switch = child:FindFirstChildOfClass("TextButton")
                    if switch then
                        switch.BackgroundColor3 = AntiLagEnabled and Color3.fromRGB(170,0,255) or Color3.fromRGB(40,40,40)
                        local ball = switch:FindFirstChildOfClass("Frame")
                        if ball then
                            ball.Position = AntiLagEnabled and UDim2.new(1,-20,0.5,-9) or UDim2.new(0,2,0.5,-9)
                        end
                    end
                end
            end
        end
    end
end)

--------------------------------------------------
-- APLICAR SPEED, JUMP E ATUALIZAR HELICÓPTERO
--------------------------------------------------
RunService.Heartbeat:Connect(function()
	if not HRP or not Humanoid then return end
	
	-- Aplicar Speed CORRETAMENTE
	local spd = 0
	if SpeedEnabled1 then spd = SpeedValue1 end
	if SpeedEnabled2 then spd = SpeedValue2 end
	
	if spd > 0 then
		local dir = Humanoid.MoveDirection
		HRP.AssemblyLinearVelocity = Vector3.new(dir.X*spd, HRP.AssemblyLinearVelocity.Y, dir.Z*spd)
	end

	-- Aplicar Jump (só se estiver ativado)
	if JumpEnabled and UserInputService:IsKeyDown(Enum.KeyCode.Space) then
		HRP.AssemblyLinearVelocity = Vector3.new(HRP.AssemblyLinearVelocity.X, 50, HRP.AssemblyLinearVelocity.Z)
	end
	
	-- Atualizar velocidade do Helicóptero se estiver ativado
	if HelicopteroEnabled and bodyAngularVelocity then
		bodyAngularVelocity.AngularVelocity = Vector3.new(0, HelicopteroSpeed, 0)
	end
end)

-- TOGGLE F PARA SPEED (ALTERNAR ENTRE SPEED 1 E SPEED 2)
UserInputService.InputBegan:Connect(function(input, gpe)
	if gpe then return end
	if input.KeyCode == Enum.KeyCode.F then
		-- Alternar entre os dois speeds
		if not SpeedEnabled1 and not SpeedEnabled2 then
			-- Se nenhum estiver ativo, ativa o Speed 1
			SpeedEnabled1 = true
			SpeedEnabled2 = false
		elseif SpeedEnabled1 then
			-- Se Speed 1 estiver ativo, muda para Speed 2
			SpeedEnabled1 = false
			SpeedEnabled2 = true
		elseif SpeedEnabled2 then
			-- Se Speed 2 estiver ativo, muda para Speed 1
			SpeedEnabled2 = false
			SpeedEnabled1 = true
		end
		
		-- Atualiza a UI do hub
		UpdateSpeedUI()
	end
end)

--------------------------------------------------
-- ABAS
--------------------------------------------------
local Tabs = { 
	COMBAT = AutoBatFrame, 
	MOVEMENT = MovementFrame,
	VISUALS = VisualsFrame,
	MISC = MiscFrame
}

for _,b in pairs(Menu:GetChildren()) do
	if b:IsA("TextButton") then
		b.MouseButton1Click:Connect(function()
			for _,f in pairs(Tabs) do f.Visible=false end
			if Tabs[b.Text] then Tabs[b.Text].Visible=true end
		end)
	end
end

--------------------------------------------------
-- DRAG
--------------------------------------------------
local dragging,dragStart,startPos
Main.InputBegan:Connect(function(i)
	if i.UserInputType==Enum.UserInputType.MouseButton1 then
		dragging=true
		dragStart=i.Position
		startPos=Main.Position
		i.Changed:Connect(function()
			if i.UserInputState==Enum.UserInputState.End then dragging=false end
		end)
	end
end)

UserInputService.InputChanged:Connect(function(i)
	if dragging and i.UserInputType==Enum.UserInputType.MouseMovement then
		local d=i.Position-dragStart
		Main.Position=UDim2.new(startPos.X.Scale,startPos.X.Offset+d.X,startPos.Y.Scale,startPos.Y.Offset+d.Y)
	end
end)

--------------------------------------------------
-- MINIMIZAR HUB
--------------------------------------------------
local Minimized = false
local FullSize = Main.Size

local MinButton = Instance.new("TextButton", Main)
MinButton.Size = UDim2.new(0,32,0,32)
MinButton.Position = UDim2.new(1,-40,0,8)
MinButton.BackgroundTransparency = 1
MinButton.Text = "-"
MinButton.Font = Enum.Font.GothamBold
MinButton.TextSize = 26
MinButton.TextColor3 = Color3.fromRGB(170,0,255)

MinButton.MouseButton1Click:Connect(function()
	Minimized = not Minimized
	if Minimized then
		MinButton.Text = "+"
		Main.Size = UDim2.new(0,600,0,55)
		Sub.Visible = false
		TitleLine.Visible = false
		Menu.Visible = false
		VLine.Visible = false
		for _,frame in pairs(Tabs) do frame.Visible = false end
	else
		MinButton.Text = "-"
		Main.Size = FullSize
		Sub.Visible = true
		TitleLine.Visible = true
		Menu.Visible = true
		VLine.Visible = true
	end
end)

-- Inicializa a UI
UpdateSpeedUI()

print("🎮 FLUXO HUB CARREGADO COM SUCESSO!")
print("📌 Discord: discord.gg/G7upZYU58Q")
print("🔥 Funcionalidades:")
print("   ✅ AUTO BAT (Combat)")
print("   ✅ SPEED x2 (Movement)")
print("   ✅ JUMP (Movement)")
print("   ✅ HELICOPTERO (Movement)")
print("   ✅ ANT RAGDOLL (Movement)")
print("   ✅ XRAY (Visuals)")
print("   ✅ ESP ALL (Visuals)")
print("   ✅ TIMER ESP (Visuals)")
print("   ✅ ANTI LAG (Visuals)")
print("   ✅ AUTO GRAB (Misc)")
print("🎮 Teclas:")
print("   F - Alternar Speed")
print("   H - Anti Ragdoll")
print("   L - Anti Lag")
print("   G - Auto Grab")
