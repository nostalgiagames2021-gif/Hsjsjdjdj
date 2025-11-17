-- ClientMenu.lua (LocalScript em StarterPlayerScripts)
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer
local event = ReplicatedStorage:WaitForChild("RequestGrant")

-- Se quiser limitar a GUI apenas para admins visuais, mantenha uma lista de usernames/ids aqui também.
-- NOTE: isto é só para esconder o botão no cliente; a verificação real é no servidor.
local VISIBLE_ADMINS = {
    ["SeuUserName"] = true, -- substitua
}

-- Cria uma GUI simples programaticamente
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "AdminModMenu"
screenGui.ResetOnSpawn = false
screenGui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 220, 0, 120)
frame.Position = UDim2.new(0, 10, 0, 10)
frame.BackgroundTransparency = 0.3
frame.BorderSizePixel = 0
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Admin Mod Menu"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 18
title.Parent = frame

local nameBox = Instance.new("TextBox")
nameBox.PlaceholderText = "Nome do jogador (vazio = a você)"
nameBox.Size = UDim2.new(1, -16, 0, 30)
nameBox.Position = UDim2.new(0, 8, 0, 36)
nameBox.Parent = frame

local giveBtn = Instance.new("TextButton")
giveBtn.Size = UDim2.new(1, -16, 0, 28)
giveBtn.Position = UDim2.new(0, 8, 0, 72)
giveBtn.Text = "Conceder Brainrot"
giveBtn.Font = Enum.Font.SourceSans
giveBtn.TextSize = 16
giveBtn.Parent = frame

-- opcional: esconder GUI para não-admins (apenas UX; servidor faz verificação real)
if not VISIBLE_ADMINS[player.Name] then
    frame.Visible = false
end

giveBtn.MouseButton1Click:Connect(function()
    local targetName = nameBox.Text
    -- envia solicitação ao servidor; servidor verifica se quem pediu é admin
    event:FireServer(targetName)
end)
