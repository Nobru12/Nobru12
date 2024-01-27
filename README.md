local ReplicatedStorage = game:GetService("ReplicatedStorage")

-- Assuming you have a RemoteEvent named "RequestFocusOnPlayerHead"
local RequestFocusOnPlayerHead = ReplicatedStorage:WaitForChild("RequestFocusOnPlayerHead")

-- Function to find the nearest player
local function findNearestPlayer()
    local localPlayer = game.Players.LocalPlayer
    local nearestPlayer = nil
    local shortestDistance = math.huge

    for _, player in ipairs(game.Players:GetPlayers()) do
        if player ~= localPlayer then
            local character = player.Character
            if character then
                local distance = (localPlayer.Character.Head.Position - character.Head.Position).Magnitude
                if distance < shortestDistance then
                    shortestDistance = distance
                    nearestPlayer = player
                end
            end
        end
    end

    return nearestPlayer
end

-- Function to focus on the nearest player's head
local function focusOnNearestPlayerHead()
    local nearestPlayer = findNearestPlayer()
    if nearestPlayer then
        -- Request focus on the nearest player's head
        RequestFocusOnPlayerHead:FireServer(nearestPlayer.Name)
    else
        -- Focus on the local player's head
        local camera = workspace.CurrentCamera
        camera.CameraSubject = game.Players.LocalPlayer.Character.Head
    end
end

-- Check for the nearest player every 2 seconds
while true do
    wait(2)
    focusOnNearestPlayerHead()
end
