-- LocalScriptをStarterPlayerScriptsに配置

-- 変数定義
local Players = game:GetService("Players")

local function createProfileGui(player)
	local character = player.Character or player.CharacterAdded:Wait()
	if not character then return end
	
	local head = character:WaitForChild("Head", 5)
	if not head then return end
	
	-- 既存のGUIがあれば削除
	if head:FindFirstChild("NameGui") then
		head:FindFirstChild("NameGui"):Destroy()
	end
	if head:FindFirstChild("IconGui") then
		head:FindFirstChild("IconGui"):Destroy()
	end
	
	-- ユーザーのサムネイルを非同期で取得
	local content = Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size150x150)

	-- ===================================
	-- 名前表示BillboardGuiの作成
	-- ===================================
	local nameGui = Instance.new("BillboardGui")
	nameGui.Name = "NameGui"
	nameGui.Size = UDim2.new(0, 400, 0, 80)
	nameGui.StudsOffset = Vector3.new(0, 7, 0)
	nameGui.AlwaysOnTop = true
	nameGui.Adornee = head
	nameGui.Parent = head
	nameGui.ExtentsOffsetWorldSpace = Vector3.new(0, 0, -20)

	local nameLabel = Instance.new("TextLabel")
	nameLabel.Name = "UsernameLabel"
	nameLabel.Size = UDim2.new(1, 0, 1, 0)
	nameLabel.Text = player.Name
	nameLabel.TextColor3 = Color3.new(1, 1, 1)
	nameLabel.Font = Enum.Font.SourceSansBold
	nameLabel.BackgroundTransparency = 1
	nameLabel.TextScaled = false
	nameLabel.TextSize = 20
	nameLabel.Position = UDim2.new(0.5, 0, 0.5, 0)
	nameLabel.AnchorPoint = Vector2.new(0.5, 0.5)
	nameLabel.Parent = nameGui
	
	local textStroke = Instance.new("UIStroke")
	textStroke.Thickness = 4
	textStroke.Color = Color3.new(0, 0, 0)
	textStroke.Parent = nameLabel
	
	-- ===================================
	-- アイコン表示BillboardGuiの作成
	-- ===================================
	local iconGui = Instance.new("BillboardGui")
	iconGui.Name = "IconGui"
	iconGui.Size = UDim2.new(0, 37.5, 0, 37.5)
	iconGui.StudsOffset = Vector3.new(0, 9.5, 0) -- ★★ ここを変更：名前からの距離を広げる
	iconGui.AlwaysOnTop = true
	iconGui.Adornee = head
	iconGui.Parent = head
	iconGui.ExtentsOffsetWorldSpace = Vector3.new(0, 0, -20)

	local imageLabel = Instance.new("ImageLabel")
	imageLabel.Name = "ProfileImage"
	imageLabel.Size = UDim2.new(1, 0, 1, 0)
	imageLabel.Image = content
	imageLabel.BackgroundColor3 = Color3.new(0.1, 0.1, 0.1)
	
	local uiCorner = Instance.new("UICorner")
	uiCorner.CornerRadius = UDim.new(0.5, 0)
	uiCorner.Parent = imageLabel
	imageLabel.BorderSizePixel = 3
	imageLabel.BorderColor3 = Color3.new(1, 1, 1)
	imageLabel.Parent = iconGui

end

-- 参加済みの全プレイヤーに対して実行
for _, player in ipairs(Players:GetPlayers()) do
	createProfileGui(player)
end

-- 新しく参加したプレイヤーに対して実行
Players.PlayerAdded:Connect(function(player)
	player.CharacterAdded:Connect(function()
		createProfileGui(player)
	end)
end)
