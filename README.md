Script below is a Roblox Inventory System. Use according to steps.

{SCRIPT BELOW MUST BE PASTED IN A ModuleScript in ServerStorage}



local InventoryModule = {}
local PlayerInventories = {}

function InventoryModule:GetInventory(player)
	if not PlayerInventories[player.UserId] then
		PlayerInventories[player.UserId] = {}
	end
	return PlayerInventories[player.UserId]
end

function InventoryModule:AddItem(player, itemName, quantity)
	local inventory = self:GetInventory(player)
	if inventory[itemName] then
		inventory[itemName] = inventory[itemName] + quantity
	else
		inventory[itemName] = quantity
	end
	return inventory
end

function InventoryModule:RemoveItem(player, itemName, quantity)
	local inventory = self:GetInventory(player)
	if inventory[itemName] then
		inventory[itemName] = inventory[itemName] - quantity
		if inventory[itemName] <= 0 then
			inventory[itemName] = nil
		end
	end
	return inventory
end

return InventoryModule




{Inventory Handler put this in a script within ServerScriptService)

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local ServerStorage = game:GetService("ServerStorage")

local AddItemEvent = ReplicatedStorage:WaitForChild("AddItemEvent")
local GetInventoryEvent = ReplicatedStorage:WaitForChild("GetInventoryEvent")

local InventoryModule = require(ServerStorage:WaitForChild("InventoryModule"))

-- When player joins, create their inventory in memory
Players.PlayerAdded:Connect(function(player)
	InventoryModule:GetInventory(player)
end)

Players.PlayerRemoving:Connect(function(player)
	-- Optional: save inventory to datastore here
end)

-- Add an item to the player's inventory
AddItemEvent.OnServerEvent:Connect(function(player, itemName, quantity)
	local inventory = InventoryModule:AddItem(player, itemName, quantity)
	print(player.Name .. " now has: ")
	for item, count in pairs(inventory) do
		print(item, count)
	end
end)

-- Get a player's inventory (returns to client)
GetInventoryEvent.OnServerInvoke = function(player)
	return InventoryModule:GetInventory(player)
end



{Developer note: Use 

AddItemEven 'RemoteEvent' Fired by the client to add an item to the player’s inventory.

GetInventoryEvent 'RemoteFunction' Invoked by the client to get the full inventory table
 
Required for script to work .Both RemoteEvent and RemoteFunction should be placed within RepStorage.}


{Add script within StarterGui LocalScript} Might not work tho current under testing phase.}


local ReplicatedStorage = game:GetService("ReplicatedStorage")
local AddItemEvent = ReplicatedStorage:WaitForChild("AddItemEvent")
local GetInventoryEvent = ReplicatedStorage:WaitForChild("GetInventoryEvent")



AddItemEvent:FireServer("Sword", 5)



local inventory = GetInventoryEvent:InvokeServer()
for item, count in pairs(inventory) do
	print(item, count)

end



Edit out for your plans and have some fun gang. Made possible by  DevNestDeveloper Jeffhezos123 gang! ALL ARE IN DIFFERENT SECTIONS OF STUDIO READ BEFORE PASTING DONKEYS!!!!!
