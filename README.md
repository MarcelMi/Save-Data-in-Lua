# Save-Data-in-Lua

local DataStoreService = game:GetService("DataStoreService")
local myDataStore = DataStoreService:GetDataStore("MyDataStore")

game.Players.PlayerAdded:Connect(function(player)
	local leaderstats = Instance.new("Folder")
	leaderstats.Name = "leaderstats"
	leaderstats.Parent = player

	local Money = Instance.new("IntValue")
	Money.Name = "Money"
	Money.Parent = leaderstats

	local data
	local success, errorMessage = pcall(function()
		data = myDataStore:GetAsync(player.UserId)
	end)

	if success then
		Money.Value = data
	else
		warn(errorMessage)
	end
end)

game.Players.PlayerRemoving:Connect(function(player)
	local success, errorMessage = pcall(function()
		local data = myDataStore:SetAsync(player.UserId, player.leaderstats.Money.Value)
	end)

	if success then
		print("Data Saved")
	else
		warn(errorMessage)
	end
end)

game:BindToClose(function()
	for i, player in pairs(game.Players:GetChildren()) do
		player:Kick("Server Closed")
	end

	wait()
end)
