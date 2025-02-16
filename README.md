
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local flying = false
local speed = 50 -- Velocidade de voo


local function startFlying()
    if not flying then
        flying = true
        
        
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.MaxForce = Vector3.new(5000, 5000, 5000) -- Força do voo
        bodyVelocity.Velocity = Vector3.new(0, speed, 0) -- Movimentação inicial para cima
        bodyVelocity.Parent = character:WaitForChild("HumanoidRootPart")
        
        
        game:GetService("RunService").Heartbeat:Connect(function()
            if flying then
                bodyVelocity.Velocity = Vector3.new(
                    humanoid.MoveDirection.X * speed,
                    bodyVelocity.Velocity.Y, 
                    humanoid.MoveDirection.Z * speed
                )
            end
        end)
    end
end


local function stopFlying()
    flying = false
    -- Remover o BodyVelocity
    local bodyVelocity = character:FindFirstChildOfClass("BodyVelocity")
    if bodyVelocity then
        bodyVelocity:Destroy()
    end
end


local function teleportToPlayerHouse(targetPlayer)
    -- Verificar se o jogador tem uma casa e teleportar
    local house = targetPlayer:FindFirstChild("House") -- Aqui, "House" seria um modelo/objeto onde a casa do jogador é armazenada
    if house then
        character:SetPrimaryPartCFrame(house.CFrame + Vector3.new(0, 5, 0)) -- Teleportar um pouco acima para evitar colisão
    else
        warn("A casa do jogador não foi encontrada.")
    end
end


local function setupGui()
    local gui = Instance.new("ScreenGui")
    gui.Name = "HeitorHub" 
    gui.Parent = player.PlayerGui
    
    
    local flyButton = Instance.new("TextButton")
    flyButton.Size = UDim2.new(0, 200, 0, 50)
    flyButton.Position = UDim2.new(0.5, -100, 0.8, 0)
    flyButton.Text = "Voar"
    flyButton.Parent = gui
    
    flyButton.MouseButton1Click:Connect(function()
        if flying then
            stopFlying()
            flyButton.Text = "Voar"
        else
            startFlying()
            flyButton.Text = "Parar de Voar"
        end
    end)
    
    
