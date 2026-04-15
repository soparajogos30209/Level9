local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local espEnabled = false
local espObjects = {}

-- Toggle com tecla (ex: "E")
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
	if gameProcessed then return end
	if input.KeyCode == Enum.KeyCode.E then
		espEnabled = not espEnabled
		
		for _, box in pairs(espObjects) do
			box.Visible = espEnabled
		end
	end
end)

-- Criar ESP
local function createESP(player)
	if player == Players.LocalPlayer then return end
	
	local box = Drawing.new("Square")
	box.Color = Color3.fromRGB(255, 0, 0)
	box.Thickness = 2
	box.Filled = false
	box.Visible = false
	
	espObjects[player] = box

	RunService.RenderStepped:Connect(function()
		if not espEnabled then return end
		
		local character = player.Character
		if character and character:FindFirstChild("HumanoidRootPart") then
			local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(character.HumanoidRootPart.Position)
			
			if onScreen then
				local size = 1000 / pos.Z
				
				box.Size = Vector2.new(size, size * 1.5)
				box.Position = Vector2.new(pos.X - box.Size.X/2, pos.Y - box.Size.Y/2)
				box.Visible = true
			else
				box.Visible = false
			end
		else
			box.Visible = false
		end
	end)
end

-- Aplicar em jogadores
for _, player in pairs(Players:GetPlayers()) do
	createESP(player)
end

Players.PlayerAdded:Connect(createESP)# Level9
