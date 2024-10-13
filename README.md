-- Referências
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Função para desenhar a linha
local function drawLine(startPos, endPos, color)
    local line = Instance.new("LineHandleAdornment")
    line.Adornee = nil -- Desenhar no mundo
    line.Color3 = color
    line.Transparency = 0
    line.Length = (endPos - startPos).magnitude
    line.ZIndex = 5
    line.CFrame = CFrame.new(startPos, endPos) * CFrame.new(0, 0, -line.Length / 2) -- Posiciona a linha
    line.Parent = workspace
end

-- Função para verificar aliados e inimigos
local function updateLines()
    for _, player in ipairs(Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local rootPart = player.Character.HumanoidRootPart
            for _, target in ipairs(Players:GetPlayers()) do
                if target ~= player and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
                    local targetRootPart = target.Character.HumanoidRootPart
                    local color = player.Team == target.Team and Color3.fromRGB(255, 255, 255) or Color3.fromRGB(255, 0, 0) -- Branco para aliados, vermelho para inimigos
                    drawLine(rootPart.Position, targetRootPart.Position, color)
                end
            end
        end
    end
end

-- Atualizar as linhas a cada frame
RunService.RenderStepped:Connect(updateLines)
