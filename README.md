local ArrayField = loadstring(game:HttpGet('https://raw.githubusercontent.com/UI-Interface/ArrayField/main/Source.lua'))()
getgenv().SecureMode = true

-- Rayfield 라이브러리 로드
local Rayfield = loadstring(game:HttpGet(('https://raw.githubusercontent.com/Ddddddadcbw/ddad/refs/heads/main/README.md')))()
-- Rayfield 윈도우 생성
local Window = Rayfield:CreateWindow({
    Name = "Whale Hub",
    LoadingTitle = "Whale",
    LoadingSubtitle = "by Whale Hub Devloper team",
    ConfigurationSaving = {
        Enabled = false,
        FolderName = nil,  -- 기본 폴더 사용
        FileName = "IWV Hub"
    },
    KeySystem = false, -- 기본 키 시스템 비활성화
    KeySettings = {
        Title = "IWV",
        Subtitle = "Key System",
        Note = "IWV Script 2024-08-31\nACS Engine & CE Engine 부대테러",
        FileName = "Key",
        SaveKey = false,
        GrabKeyFromSite = false,
        Key = {[1] = "12345678"} -- 이 필드는 비워두고 직접 검증 로직 구현
    }
})
---------------------- 기본기능 함수 모음 -----------------------
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera

-- 밥밥부대 탈옥감지 무력화
local lineObject = game:GetService("ReplicatedStorage"):FindFirstChild("line")

if lineObject then
    lineObject:Destroy() -- line 객체를 제거합니다.
end

-- 플래그 및 상태 변수
local isKeyValid = false
local dragging = false
local dragInput, mousePos, framePos
local teleporting = false
local isFlying = false
local spinSpeed = 2
local flySpeed = 2
local spinning = false
local noclip = false
local userName = ""
local espEnabled = false
local espObjects = {}

-- 기능 플래그
local CurrentValue = false
local flyEnabled = false
local noclipEnabled = false
local spinEnabled = false
local KillAuraEnabled = false
local AimBotEnabled = false
local StaticCuffEnabled = false
local TpKillEnabled = false
local swordAttackEnabled = false -- swordattack 기능 상태
local increaseSize = false -- 작대기 크기 증가 상태
local swordEquipEnabled = false  -- 검 꺼내기 상태
local swordSizeIncreaseEnabled = false  -- 검 크기 키우기 상태

-- Aimbot 관련 변수
local aimAssistEnabled = false
local currentTarget = nil
local renderSteppedConnection
local frame = nil
local uiStroke = nil
local colors = {
    Color3.fromRGB(255, 0, 0),
    Color3.fromRGB(255, 165, 0),
    Color3.fromRGB(255, 255, 0),
    Color3.fromRGB(0, 255, 0),
    Color3.fromRGB(0, 0, 255),
    Color3.fromRGB(128, 0, 128),
    Color3.fromRGB(0, 0, 0)
}
local baseSize = 100
local sizeIncrement = 50
local currentColorIndex = 1

local ToggleFly, ToggleNoclip, ToggleESP, ToggleSpin, ToggleKillAura, ToggleAimBot, ToggleStaticCuff, ToggleTpKill
local ToggleWeaponSize, ToggleSwordAttack

------------------------ 키 인증 부분 -------------------------------
-- Pastebin 링크
local dataUrl = 'https://pastebin.com/raw/HFHDpMQR'

-- 데이터를 가져오는 코드
local success, response = pcall(function()
    local jsonData = game:HttpGet(dataUrl)
    return HttpService:JSONDecode(jsonData)
end)

if success then
    keysData = response
else
    warn("Failed to fetch JSON data: " .. tostring(response))
end
------------------------ 키 인증 부분 -------------------------------
local Players = game:GetService("Players")

-- 함수: 유저 ID나 유저 닉네임의 일부를 통해 플레이어의 디스플레이 닉네임을 찾습니다.
local function findPlayerDisplayName(identifier)
    for _, player in ipairs(Players:GetPlayers()) do
        -- 유저 닉네임의 일부로 검색 (identifier가 문자열일 때)
        if string.find(string.lower(player.Name), string.lower(identifier)) then
            return player.Name
        end
    end
end
------------------------ 강철 부대 ----------------------------
local function gangchulCuff()
    local ohString1 = "Cuff"
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game:GetService("Players").LocalPlayer and player then
            local CuffItem = player.Backpack:FindFirstChild(utf8.char(49688, 44049))
            if CuffItem then
                if not CuffItem:FindFirstChild("RemoteEvent") then
                    local RemoteEvent = Instance.new("RemoteEvent")
                    RemoteEvent.Name = "RemoteEvent"
                    RemoteEvent.Parent = CuffItem
                end
                CuffItem.RemoteEvent:FireServer(ohString1, player.Character.HumanoidRootPart)
            end
        end
    end
end

local function gangchulUnCuff()
    local ohString1 = "UnCuff"
    for _, player in pairs(game:GetService("Players"):GetPlayers()) do
        if player ~= game:GetService("Players").LocalPlayer and player then
            local CuffItem = player.Backpack:FindFirstChild(utf8.char(49688, 44049))
            if CuffItem then
                if not CuffItem:FindFirstChild("RemoteEvent") then
                    local RemoteEvent = Instance.new("RemoteEvent")
                    RemoteEvent.Name = "RemoteEvent"
                    RemoteEvent.Parent = CuffItem
                end
                CuffItem.RemoteEvent:FireServer(ohString1, player.Character.HumanoidRootPart)
            end
        end
    end
end

------------------------ 프리즌 라이프 -------------------------------
local function Kick_Server()
    -- AK-47 아이템을 workspace에서 찾음
    local akItem = workspace[game.Players.LocalPlayer.Name]:FindFirstChild("AK-47")
    if not akItem then
        warn("AK-47 not found in workspace")
        return
    end

    local gunStatesModule = akItem:FindFirstChild("GunStates")
    if not gunStatesModule then
        warn("GunStates ModuleScript not found in AK-47")
        return
    end

    -- GunStates 모듈스크립트에서 반환된 테이블 수정
    local gunStates = require(gunStatesModule)
    gunStates.Damage = 1000
    gunStates.Description = "Remember to put a description here BEFORE the game is published -Me"
    gunStates.MaxAmmo = 100000000
    gunStates.CurrentAmmo = 10000000
    gunStates.StoredAmmo = 600
    gunStates.FireRate = 0.00000001
    gunStates.AutoFire = true
    gunStates.Range = 100000000
    gunStates.Spread = 14
    gunStates.ReloadTime = 0.01
    gunStates.Bullets = 0
    gunStates.ReloadAnim = "ReloadMagazine"
    gunStates.ShootAnim = "ShootBullet"
    gunStates.HoldAnim = "Hold"
    gunStates.FireSoundId = "http://www.roblox.com/asset/?id=2934888736"
    gunStates.SecondarySoundId = nil
    gunStates.ReloadSoundId = "http://www.roblox.com/asset/?id=2934887229"

    -- 수정된 GunStates 테이블을 ReloadEvent로 서버에 전달
    game:GetService("ReplicatedStorage").ReloadEvent:FireServer(akItem)
end

local function teleport_f()
    local character = Player.Character or Player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    local targetPosition = Vector3.new(-936, 93, 2056)
    local originalPosition = humanoidRootPart.Position
    local function teleportToPosition(position)
        humanoidRootPart.CFrame = CFrame.new(position)
    end
    local function hasAK47()
        for _, item in pairs(Player.Backpack:GetChildren()) do
            if item.Name == "AK-47" then
                return true
            end
        end
        return false
    end
    local function performAction()
        local ak47Item = workspace.Prison_ITEMS.giver:FindFirstChild("AK-47")
        if ak47Item and ak47Item:IsA("Model") and ak47Item:FindFirstChild("ITEMPICKUP") then
            local args = { ak47Item.ITEMPICKUP }
            workspace.Remote.ItemHandler:InvokeServer(unpack(args))
        else
            warn("AK-47 item not found or ITEMPICKUP not available.")
        end
    end
    local function returnToOriginalPosition()
        humanoidRootPart.CFrame = CFrame.new(originalPosition)
    end
    local function teleportAndPerformAction()
        teleportToPosition(targetPosition)
        wait(0.1)
        while not hasAK47() do
            performAction()
            wait(0.1)
        end
        returnToOriginalPosition()
    end
    teleportAndPerformAction()
end

local function getTargetPlayers()
    local targetPlayers = {}
    for _, player in pairs(Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            if player ~= Player then
                table.insert(targetPlayers, player)
            end
        end
    end
    return targetPlayers
end

-- 공격 루프 관리 변수
local attacking = false

-- 공격 함수
local function attackAllPlayers(onoff)
    -- ReplicatedStorage와 공격 이벤트 가져오기
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local meleeEvent = ReplicatedStorage:WaitForChild("meleeEvent")

    -- 공격 시작 또는 중지 설정
    attacking = onoff

    if not attacking then
        return -- 공격 중지
    end

    -- 대상 플레이어 목록 가져오기
    local players = game:GetService("Players"):GetPlayers()
    local localPlayer = game:GetService("Players").LocalPlayer
    local character = localPlayer.Character

    if not (character and character:FindFirstChild("HumanoidRootPart")) then
        return -- 로컬 플레이어가 유효하지 않을 경우 종료
    end

    -- 모든 플레이어를 대상으로 공격 루프
    while true do
        if not attacking then
            break -- 공격 중지
        end

        for _, player in pairs(players) do
            if not attacking then
                break -- 공격 중지
            end

            if player ~= localPlayer and player.Character and player.Character:FindFirstChild("Humanoid") then
                local targetCharacter = player.Character
                local targetHumanoid = targetCharacter.Humanoid

                if targetHumanoid.Health > 0 and targetCharacter:FindFirstChild("HumanoidRootPart") then
                    -- 로컬 플레이어를 타겟 플레이어 근처로 텔레포트
                    character.HumanoidRootPart.CFrame = targetCharacter.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3) -- 약간 뒤로 이동

                    -- 공격 이벤트 발송
                    meleeEvent:FireServer(player)
                end
            end
        end
        wait(0.009) -- 이벤트 발송 간격
    end

    -- 공격 종료
    attacking = false
end
------------------------ 프리즌 라이프 -------------------------------
--------------------------------------- 은신 기능 ----------------------------------------------------
-- 서비스와 플레이어 참조
local players = game:GetService("Players")
local runService = game:GetService("RunService")
local userInputService = game:GetService("UserInputService")
local localPlayer = players.LocalPlayer
local camera = workspace.CurrentCamera

function Transparency_toggle_bt(value)
    --Settings:
    local ScriptStarted = false
    local Keybind = "H" --Set to whatever you want, has to be the name of a KeyCode Enum.
    local Transparency = true --Will make you slightly transparent when you are invisible. No reason to disable.
    local NoClip = false --Will make your fake character no clip.

    local Player = game:GetService("Players").LocalPlayer
    local RealCharacter = Player.Character or Player.CharacterAdded:Wait()

    local IsInvisible = false

    RealCharacter.Archivable = true
    local FakeCharacter = RealCharacter:Clone()
    local Part
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(0, -500, 0) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
    if v:IsA("LocalScript") then
        local clone = v:Clone()
        clone.Disabled = true
        clone.Parent = FakeCharacter
    end
    end
    if Transparency then
    for i, v in pairs(FakeCharacter:GetDescendants()) do
        if v:IsA("BasePart") then
            v.Transparency = 0.7
        end
    end
    end
    local CanInvis = true
    function RealCharacterDied()
    CanInvis = false
    RealCharacter:Destroy()
    RealCharacter = Player.Character
    CanInvis = true
    isinvisible = false
    FakeCharacter:Destroy()
    workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid

    RealCharacter.Archivable = true
    FakeCharacter = RealCharacter:Clone()
    Part:Destroy()
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(9999, 9999, 9999) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
        if v:IsA("LocalScript") then
            local clone = v:Clone()
            clone.Disabled = true
            clone.Parent = FakeCharacter
        end
    end
    if Transparency then
        for i, v in pairs(FakeCharacter:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 0.7
            end
        end
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    local PseudoAnchor
    game:GetService "RunService".RenderStepped:Connect(
    function()
        if PseudoAnchor ~= nil then
            PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0)
        end
        if NoClip then
        FakeCharacter.Humanoid:ChangeState(11)
        end
    end
    )

    PseudoAnchor = FakeCharacter.HumanoidRootPart
    local function Invisible()
    if IsInvisible == false then
        local StoredCF = RealCharacter.HumanoidRootPart.CFrame
        RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = StoredCF
        RealCharacter.Humanoid:UnequipTools()
        Player.Character = FakeCharacter
        workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
        PseudoAnchor = RealCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = false
            end
        end

        IsInvisible = true
    else
        local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
        
        RealCharacter.HumanoidRootPart.CFrame = StoredCF
        
        FakeCharacter.Humanoid:UnequipTools()
        Player.Character = RealCharacter
        workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
        PseudoAnchor = FakeCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = true
            end
        end
        IsInvisible = false
    end
    end

    game:GetService("UserInputService").InputBegan:Connect(
    function(key, gamep)
        if gamep then
            return
        end
        if key.KeyCode.Name:lower() == Keybind:lower() and CanInvis and RealCharacter and FakeCharacter then
            if RealCharacter:FindFirstChild("HumanoidRootPart") and FakeCharacter:FindFirstChild("HumanoidRootPart") then
                Invisible()
            end
        end
    end
    )
    local Sound = Instance.new("Sound",game:GetService("SoundService"))
    Sound.SoundId = "rbxassetid://232127604"
    Sound:Play()
    game:GetService("StarterGui"):SetCore("SendNotification",{["Title"] = "Invisible Toggle Loaded",["Text"] = "Press "..Keybind.." to become change visibility.",["Duration"] = 20,["Button1"] = "Okay."})
end
--------------------------------------- 은신 기능 ----------------------------------------------------
--kill용 은신
function Transparency_toggle_bt_kill()
    --Settings:
    local ScriptStarted = false
    local Keybind = "H" --Set to whatever you want, has to be the name of a KeyCode Enum.
    local Transparency = true --Will make you slightly transparent when you are invisible. No reason to disable.
    local NoClip = false --Will make your fake character no clip.

    local Player = game:GetService("Players").LocalPlayer
    local RealCharacter = Player.Character or Player.CharacterAdded:Wait()

    local IsInvisible = false

    RealCharacter.Archivable = true
    local FakeCharacter = RealCharacter:Clone()
    local Part
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(0, -1000, 0) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
    if v:IsA("LocalScript") then
        local clone = v:Clone()
        clone.Disabled = true
        clone.Parent = FakeCharacter
    end
    end
    if Transparency then
    for i, v in pairs(FakeCharacter:GetDescendants()) do
        if v:IsA("BasePart") then
            v.Transparency = 0.7
        end
    end
    end
    local CanInvis = true
    function RealCharacterDied()
    CanInvis = false
    RealCharacter:Destroy()
    RealCharacter = Player.Character
    CanInvis = true
    isinvisible = false
    FakeCharacter:Destroy()
    workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid

    RealCharacter.Archivable = true
    FakeCharacter = RealCharacter:Clone()
    Part:Destroy()
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(9999, 9999, 9999) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
        if v:IsA("LocalScript") then
            local clone = v:Clone()
            clone.Disabled = true
            clone.Parent = FakeCharacter
        end
    end
    if Transparency then
        for i, v in pairs(FakeCharacter:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 0.7
            end
        end
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    local PseudoAnchor
    game:GetService "RunService".RenderStepped:Connect(
    function()
        if PseudoAnchor ~= nil then
            PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0)
        end
        if NoClip then
        FakeCharacter.Humanoid:ChangeState(11)
        end
    end
    )

    PseudoAnchor = FakeCharacter.HumanoidRootPart
    local function Invisible()
    if IsInvisible == false then
        local StoredCF = RealCharacter.HumanoidRootPart.CFrame
        RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = StoredCF
        RealCharacter.Humanoid:UnequipTools()
        Player.Character = FakeCharacter
        workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
        PseudoAnchor = RealCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = false
            end
        end

        IsInvisible = true
    else
        local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
        
        RealCharacter.HumanoidRootPart.CFrame = StoredCF
        
        FakeCharacter.Humanoid:UnequipTools()
        Player.Character = RealCharacter
        workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
        PseudoAnchor = FakeCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = true
            end
        end
        IsInvisible = false
    end
    end

    game:GetService("UserInputService").InputBegan:Connect(
    function(key, gamep)
        if gamep then
            return
        end
        if key.KeyCode.Name:lower() == Keybind:lower() and CanInvis and RealCharacter and FakeCharacter then
            if RealCharacter:FindFirstChild("HumanoidRootPart") and FakeCharacter:FindFirstChild("HumanoidRootPart") then
                Invisible()
            end
        end
    end
    )
    local Sound = Instance.new("Sound",game:GetService("SoundService"))
    Sound.SoundId = "rbxassetid://232127604"
    Sound:Play()
    game:GetService("StarterGui"):SetCore("SendNotification",{["Title"] = "Invisible Toggle Loaded",["Text"] = "Press "..Keybind.." to become change visibility.",["Duration"] = 20,["Button1"] = "Okay."})
end
---------------------- 킬용 은신 ------------------------
---------------------- 특정 플레이어 날리기 --------------------
-- 메인 함수 정의
function Select_Fling_Player(username)
    local Players = game:GetService("Players")
    local Player = Players.LocalPlayer

    local AllBool = false

    local function Message(_Title, _Text, Time)
        game:GetService("StarterGui"):SetCore("SendNotification", {Title = _Title, Text = _Text, Duration = Time})
    end

    local function GetPlayer(Name)
        Name = Name:lower()
        if Name == "all" or Name == "others" then
            AllBool = true
            return nil
        elseif Name == "random" then
            local GetPlayers = Players:GetPlayers()
            if table.find(GetPlayers, Player) then table.remove(GetPlayers, table.find(GetPlayers, Player)) end
            return GetPlayers[math.random(#GetPlayers)]
        else
            for _, x in next, Players:GetPlayers() do
                if x ~= Player then
                    if x.Name:lower():match("^" .. Name) or x.DisplayName:lower():match("^" .. Name) then
                        return x
                    end
                end
            end
        end
        return nil
    end

    local function SkidFling(TargetPlayer)
        local Character = Player.Character
        local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
        local RootPart = Humanoid and Humanoid.RootPart

        local TCharacter = TargetPlayer.Character
        local THumanoid
        local TRootPart
        local THead
        local Accessory
        local Handle

        if TCharacter and TCharacter:FindFirstChildOfClass("Humanoid") then
            THumanoid = TCharacter:FindFirstChildOfClass("Humanoid")
        end
        if THumanoid and THumanoid.RootPart then
            TRootPart = THumanoid.RootPart
        end
        if TCharacter and TCharacter:FindFirstChild("Head") then
            THead = TCharacter.Head
        end
        if TCharacter and TCharacter:FindFirstChildOfClass("Accessory") then
            Accessory = TCharacter:FindFirstChildOfClass("Accessory")
        end
        if Accessory and Accessory:FindFirstChild("Handle") then
            Handle = Accessory.Handle
        end

        if Character and Humanoid and RootPart then
            if RootPart.Velocity.Magnitude < 50 then
                getgenv().OldPos = RootPart.CFrame
            end
            if THumanoid and THumanoid.Sit and not AllBool then
                return Message("Error Occurred", "Targeting is sitting", 5)
            end
            if THead then
                workspace.CurrentCamera.CameraSubject = THead
            elseif not THead and Handle then
                workspace.CurrentCamera.CameraSubject = Handle
            elseif THumanoid and TRootPart then
                workspace.CurrentCamera.CameraSubject = THumanoid
            end
            if not TCharacter:FindFirstChildWhichIsA("BasePart") then
                return
            end

            local function FPos(BasePart, Pos, Ang)
                RootPart.CFrame = CFrame.new(BasePart.Position) * Pos * Ang
                Character:SetPrimaryPartCFrame(CFrame.new(BasePart.Position) * Pos * Ang)
                RootPart.Velocity = Vector3.new(9e7, 9e7 * 10, 9e7)
                RootPart.RotVelocity = Vector3.new(9e8, 9e8, 9e8)
            end

            local function SFBasePart(BasePart)
                local TimeToWait = 2
                local Time = tick()
                local Angle = 0

                repeat
                    if RootPart and THumanoid then
                        if BasePart.Velocity.Magnitude < 50 then
                            Angle = Angle + 100

                            FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(2.25, 1.5, -2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(-2.25, -1.5, 2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()
                        else
                            FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, -THumanoid.WalkSpeed), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, -TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(math.rad(-90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                            task.wait()
                        end
                    else
                        break
                    end
                until BasePart.Velocity.Magnitude > 500 or BasePart.Parent ~= TargetPlayer.Character or TargetPlayer.Parent ~= Players or not TargetPlayer.Character == TCharacter or THumanoid.Sit or Humanoid.Health <= 0 or tick() > Time + TimeToWait
            end

            workspace.FallenPartsDestroyHeight = 0 / 0

            local BV = Instance.new("BodyVelocity")
            BV.Name = "EpixVel"
            BV.Parent = RootPart
            BV.Velocity = Vector3.new(9e8, 9e8, 9e8)
            BV.MaxForce = Vector3.new(1 / 0, 1 / 0, 1 / 0)

            Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)

            if TRootPart and THead then
                if (TRootPart.CFrame.p - THead.CFrame.p).Magnitude > 5 then
                    SFBasePart(THead)
                else
                    SFBasePart(TRootPart)
                end
            elseif TRootPart and not THead then
                SFBasePart(TRootPart)
            elseif not TRootPart and THead then
                SFBasePart(THead)
            elseif not TRootPart and not THead and Accessory and Handle then
                SFBasePart(Handle)
            else
                return Message("Error Occurred", "Target is missing everything", 5)
            end

            BV:Destroy()
            Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
            workspace.CurrentCamera.CameraSubject = Humanoid

            repeat
                RootPart.CFrame = getgenv().OldPos * CFrame.new(0, .5, 0)
                Character:SetPrimaryPartCFrame(getgenv().OldPos * CFrame.new(0, .5, 0))
                Humanoid:ChangeState("GettingUp")
                table.foreach(Character:GetChildren(), function(_, x)
                    if x:IsA("BasePart") then
                        x.Velocity, x.RotVelocity = Vector3.new(), Vector3.new()
                    end
                end)
                task.wait()
            until (RootPart.Position - getgenv().OldPos.p).Magnitude < 25
            workspace.FallenPartsDestroyHeight = getgenv().FPDH
        else
            return Message("Error Occurred", "Random error", 5)
        end
    end

    -- Main logic to select player and fling
    local targetPlayer = GetPlayer(username)
    if targetPlayer and targetPlayer ~= Player then
        SkidFling(targetPlayer)
    else
        Message("Error Occurred", "Username Invalid or not found", 5)
    end
end
---------------------- ESP 기능 ------------------------
-- 서비스 및 변수 설정
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local Player = Players.LocalPlayer
local espEnabled = false
local espTransparency = 0.3
local espObjects = {}

-- 캐릭터의 루트 파트를 가져오는 함수
local function getRoot(character)
    return character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Head")
end

-- 숫자를 반올림하는 함수
local function round(num, numDecimalPlaces)
    local mult = 10^(numDecimalPlaces or 0)
    return math.floor(num * mult + 0.5) / mult
end

-- 플레이어에 대한 ESP를 생성하는 함수
local function createESP(plr)
    if plr == Player then return end -- 자기 자신에 대한 ESP는 생성하지 않음

    local function setupESP()
        local character = plr.Character
        if not character or not character:FindFirstChild("Humanoid") then return end

        local rootPart = getRoot(character)
        if not rootPart then return end

        local existingESP = espObjects[plr.UserId]
        if existingESP then
            existingESP:Destroy()
            espObjects[plr.UserId] = nil
        end

        local ESPholder = Instance.new("Folder")
        ESPholder.Name = plr.Name..'_ESP'
        ESPholder.Parent = CoreGui
        espObjects[plr.UserId] = ESPholder

        -- 캐릭터의 각 파트에 대해 ESP 박스 생성
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                local adornment = Instance.new("BoxHandleAdornment")
                adornment.Name = plr.Name
                adornment.Parent = ESPholder
                adornment.Adornee = part
                adornment.AlwaysOnTop = true
                adornment.ZIndex = 10
                adornment.Size = part.Size
                adornment.Transparency = espTransparency
                adornment.Color3 = plr.TeamColor.Color
            end
        end

        -- 캐릭터의 머리 위에 텍스트 GUI 생성
        local head = character:FindFirstChild("Head")
        if head then
            local BillboardGui = Instance.new("BillboardGui")
            local TextLabel = Instance.new("TextLabel")
            BillboardGui.Adornee = head
            BillboardGui.Name = plr.Name
            BillboardGui.Parent = ESPholder
            BillboardGui.Size = UDim2.new(0, 100, 0, 150)
            BillboardGui.StudsOffset = Vector3.new(0, 2, 0)
            BillboardGui.AlwaysOnTop = true
            TextLabel.Parent = BillboardGui
            TextLabel.BackgroundTransparency = 1
            TextLabel.Position = UDim2.new(0, 0, 0, -50)
            TextLabel.Size = UDim2.new(0, 100, 0, 100)
            TextLabel.Font = Enum.Font.SourceSansSemibold
            TextLabel.TextSize = 20
            TextLabel.TextColor3 = Color3.new(1, 1, 1)
            TextLabel.TextStrokeTransparency = 0
            TextLabel.TextYAlignment = Enum.TextYAlignment.Bottom
            TextLabel.Text = 'Name: '..plr.Name

            -- ESP 업데이트 함수
            local function updateESP()
                if not ESPholder.Parent then return end
                if character and rootPart and character:FindFirstChildOfClass("Humanoid") then
                    local playerRootPart = getRoot(Player.Character)
                    if playerRootPart and rootPart then
                        local distance = round((rootPart.Position - playerRootPart.Position).Magnitude, 1)
                        local health = round(character:FindFirstChildOfClass('Humanoid').Health, 1)
                        TextLabel.Text = 'Name: '..plr.Name..' | Health: '..health..' | Distance: '..distance
                    end
                end
            end

            -- RenderStepped에서 ESP 업데이트 루프 실행
            local espLoopFunc = RunService.RenderStepped:Connect(updateESP)

            -- 캐릭터가 제거될 때 ESP 및 업데이트 루프 제거
            local function onCharacterRemoving()
                espLoopFunc:Disconnect()
                if ESPholder.Parent then
                    ESPholder:Destroy()
                end
            end

            -- 새 캐릭터가 추가될 때 ESP 재설정
            local function onCharacterAdded(newCharacter)
                setupESP() -- 새로운 캐릭터가 추가되면 ESP 재설정
            end

            -- 이벤트 연결
            plr.CharacterRemoving:Connect(onCharacterRemoving)
            plr.CharacterAdded:Connect(onCharacterAdded)

            -- 팀 색상 변경 시 ESP 색상 업데이트
            local function onTeamChanged()
                for _, adornment in pairs(ESPholder:GetChildren()) do
                    if adornment:IsA("BoxHandleAdornment") then
                        adornment.Color3 = plr.TeamColor.Color
                    end
                end
            end
            plr:GetPropertyChangedSignal("TeamColor"):Connect(onTeamChanged)
        end
    end

    -- 캐릭터가 존재할 경우 ESP 설정
    if plr.Character then
        setupESP()
    end

    -- 새로 추가된 캐릭터에 대해서도 ESP 설정
    plr.CharacterAdded:Connect(setupESP)
end

-- 플레이어가 떠날 때 ESP 제거
local function removeESPForPlayer(plr)
    local esp = espObjects[plr.UserId]
    if esp then
        esp:Destroy()
        espObjects[plr.UserId] = nil
    end
end

-- 모든 플레이어에 대한 ESP 설정
local function applyESP()
    for _, plr in pairs(Players:GetPlayers()) do
        createESP(plr)
    end
end

-- 모든 플레이어에 대한 ESP 제거
local function removeESP()
    for _, esp in pairs(espObjects) do
        if esp then
            esp:Destroy()
        end
    end
    espObjects = {}
end

-- 자신의 캐릭터가 추가될 때 모든 플레이어에게 ESP 적용
Player.CharacterAdded:Connect(function()
    if espEnabled then
        applyESP()
    end
end)

-- 새로 추가된 플레이어에게 ESP 적용
Players.PlayerAdded:Connect(function(plr)
    if espEnabled then
        createESP(plr)
    end
end)

-- 플레이어가 떠날 때 ESP 제거
Players.PlayerRemoving:Connect(function(plr)
    removeESPForPlayer(plr)
end)

-- 플레이어의 캐릭터가 사망하거나 재생성될 때 ESP 업데이트
Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function(character)
        if espEnabled then
            createESP(plr)
        end
    end)
end)

-- 게임 시작 시 이미 존재하는 플레이어들에게 ESP 적용
if espEnabled then
    applyESP()
end
---------------------- ESP 기능 --------------------------
---------------------- FLY 기능 --------------------------
local function startFlying()
    if isFlying then return end
    isFlying = true
    local T = Player.Character.HumanoidRootPart
    local CONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
    local lCONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
    local SPEED = 0

    local function FLY()
        local BG = Instance.new('BodyGyro')
        local BV = Instance.new('BodyVelocity')
        BG.P = 9e4
        BG.Parent = T
        BV.Parent = T
        BG.maxTorque = Vector3.new(9e9, 9e9, 9e9)
        BG.cframe = T.CFrame
        BV.velocity = Vector3.new(0, 0, 0)
        BV.maxForce = Vector3.new(9e9, 9e9, 9e9)

        task.spawn(function()
            repeat wait()
                if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 or CONTROL.Q + CONTROL.E ~= 0 then
                    SPEED = flySpeed * 40
                elseif SPEED ~= 0 then
                    SPEED = 0
                end
                if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 or (CONTROL.Q + CONTROL.E) ~= 0 then
                    BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B + CONTROL.Q + CONTROL.E) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                    lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
                elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and (CONTROL.Q + CONTROL.E) == 0 and SPEED ~= 0 then
                    BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B + CONTROL.Q + CONTROL.E) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                else
                    BV.velocity = Vector3.new(0, 0, 0)
                end
                BG.cframe = workspace.CurrentCamera.CoordinateFrame
            until not isFlying
            CONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
            lCONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
            SPEED = 0
            BG:Destroy()
            BV:Destroy()
            if Player.Character:FindFirstChildOfClass('Humanoid') then
                Player.Character:FindFirstChildOfClass('Humanoid').PlatformStand = false
            end
        end)
    end

    local function onKeyPress(KEY)
        if KEY:lower() == 'w' then
            CONTROL.F = 1
        elseif KEY:lower() == 's' then
            CONTROL.B = -1
        elseif KEY:lower() == 'a' then
            CONTROL.L = -1
        elseif KEY:lower() == 'd' then
            CONTROL.R = 1
        elseif KEY:lower() == 'e' then
            CONTROL.Q = 1
        elseif KEY:lower() == 'q' then
            CONTROL.E = -1
        end
    end

    local function onKeyRelease(KEY)
        if KEY:lower() == 'w' then
            CONTROL.F = 0
        elseif KEY:lower() == 's' then
            CONTROL.B = 0
        elseif KEY:lower() == 'a' then
            CONTROL.L = 0
        elseif KEY:lower() == 'd' then
            CONTROL.R = 0
        elseif KEY:lower() == 'e' then
            CONTROL.Q = 0
        elseif KEY:lower() == 'q' then
            CONTROL.E = 0
        end
    end

    UserInputService.InputBegan:Connect(function(input, processed)
        if not processed and input.UserInputType == Enum.UserInputType.Keyboard then
            onKeyPress(input.KeyCode.Name)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Keyboard then
            onKeyRelease(input.KeyCode.Name)
        end
    end)

    FLY()
end

local function stopFlying()
    if not isFlying then return end
    isFlying = false
    if Player.Character:FindFirstChildOfClass('Humanoid') then
        Player.Character:FindFirstChildOfClass('Humanoid').PlatformStand = false
    end
end

local function startSpinning()
    local character = Player.Character or Player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    for _, v in pairs(humanoidRootPart:GetChildren()) do
        if v:IsA("BodyAngularVelocity") then
            v:Destroy()
        end
    end
    
    local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
    bodyAngularVelocity.Name = "Spinning"
    bodyAngularVelocity.Parent = humanoidRootPart
    bodyAngularVelocity.MaxTorque = Vector3.new(0, math.huge, 0)
    
    local angularVelocityMagnitude = math.rad(spinSpeed) * 300
    bodyAngularVelocity.AngularVelocity = Vector3.new(0, angularVelocityMagnitude, 0)
end

local function stopSpinning()
    local character = Player.Character
    if not character then return end
    
    for _, v in pairs(character.HumanoidRootPart:GetChildren()) do
        if v:IsA("BodyAngularVelocity") then
            v:Destroy()
        end
    end
end
---------------------- FLY 기능 ------------------------------
---------------------- Noclip 기능 ------------------------------
local function NoclipLoop()
    if noclip and Player.Character then
        for _, child in pairs(Player.Character:GetDescendants()) do
            if child:IsA("BasePart") and child.CanCollide then
                child.CanCollide = false
            end
        end
    end
end

local function setNoclip(value)
    noclip = value
    if noclip then
        while noclip do
            NoclipLoop()
            task.wait(0.1)
        end
    else
        for _, child in pairs(Player.Character:GetDescendants()) do
            if child:IsA("BasePart") then
                child.CanCollide = true
            end
        end
    end
end
---------------------- Noclip 기능 ------------------------------
----------------------- ACS CE -----------------------------

function ACS_ALL_KILL()
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local Event = ReplicatedStorage:WaitForChild("ACS_Engine", 3).Events.AcessId 
    local Players = game:GetService("Players")

    local key = Event:InvokeServer(Players.LocalPlayer.UserId) .. "-" .. tostring(Players.LocalPlayer.UserId)

    local gun = game:FindFirstChild("ACS_Settings", true).Parent
    local module = require(gun.ACS_Settings)

    for idx, plr in pairs(Players:GetPlayers()) do
        if plr ~= Players.LocalPlayer then
            Event:InvokeServer(gun, plr.Character.Humanoid, 25, 1, module, { minDamageMod=150, DamageMod=150 }, nil, nil, key)
        end
    end
end

-- 마우스 클릭 이벤트를 저장할 변수
local ACS_mouseClick_Breach

-- 마우스 클릭 이벤트 활성화 함수
function ACS_Click_Breach_ON(ACS_X, ACS_Y, ACS_Z)
    -- 로컬 플레이어 가져오기
    local Player = game:GetService("Players").LocalPlayer
    local Mouse = Player:GetMouse()
    
    -- 블록 설치를 요청하는 원격 함수 가져오기
    local PlaceBlockRemote = game:GetService("ReplicatedStorage"):WaitForChild("ACS_Engine"):WaitForChild("Events"):WaitForChild("Breach")

    if not ACS_mouseClick_Breach then  -- 이미 연결되어 있는지 확인
        ACS_mouseClick_Breach = Mouse.Button1Down:Connect(function()
            -- 클릭 위치 정보 가져오기
            local clickPosition = Mouse.Hit.p
            Player.Character.ACS_Client.Kit.Fortifications.Value = 1000
            -- 숫자 변환 (형변환 실패 시 기본값 1 설정)
            local x = tonumber(ACS_X) or 10
            local y = tonumber(ACS_Y) or 10
            local z = tonumber(ACS_Z) or 10

            -- 서버에 블록 설치 요청 보내기
            local args = {
                [1] = 3, -- 모드 (예: 설치 모드 ID)
                [2] = { -- BreachPlace 객체 생성
                    Fortified = { Value = true },
                    Destroyable = workspace
                },
                [3] = clickPosition, -- 클릭 위치 사용
                [4] = nil,
                [5] = { -- Hit 객체 생성 (예시)
                    Size = Vector3.new(x, y, z),
                    CFrame = CFrame.new(clickPosition)
                }
            }

            PlaceBlockRemote:InvokeServer(unpack(args))
        end)
        print("마우스 이벤트 활성화됨.")
    else
        print("이미 마우스 이벤트가 활성화되어 있습니다.")
    end
end

-- 마우스 클릭 이벤트 비활성화 함수
function ACS_Click_Breach_OFF()
    -- 로컬 플레이어 가져오기
    local Player = game:GetService("Players").LocalPlayer
    local Mouse = Player:GetMouse()
    
    -- 블록 설치를 요청하는 원격 함수 가져오기
    local PlaceBlockRemote = game:GetService("ReplicatedStorage"):WaitForChild("ACS_Engine"):WaitForChild("Events"):WaitForChild("Breach")

    if ACS_mouseClick_Breach then
        ACS_mouseClick_Breach:Disconnect()
        ACS_mouseClick_Breach = nil
        print("마우스 이벤트 비활성화됨.")
    else
        print("마우스 이벤트가 이미 비활성화되어 있습니다.")
    end
end

function ACS_Player_fli()
    -- 로컬 플레이어 가져오기
    local Player = game:GetService("Players").LocalPlayer
    local Mouse = Player:GetMouse()
    
    -- 블록 설치를 요청하는 원격 함수 가져오기
    local PlaceBlockRemote = game:GetService("ReplicatedStorage"):WaitForChild("ACS_Engine"):WaitForChild("Events"):WaitForChild("Breach")

    -- 서버에 블록 설치 요청 보내기
    local args = {
        [1] = 3, -- 모드 (예: 설치 모드 ID)
        [2] = { -- BreachPlace 객체 생성
            Fortified = { Value = true },
            Destroyable = workspace
        },
        [3] = game:GetService("Workspace").Aerilo8860.HumanoidRootPart.Position, -- 유저 위치 사용
        [4] = nil,
        [5] = { -- Hit 객체 생성 (예시)
            Size = Vector3.new(100000, 100000, 100000),
            CFrame = CFrame.new(game:GetService("Workspace").Aerilo8860.HumanoidRootPart.Position)
        }
    }

    PlaceBlockRemote:InvokeServer(unpack(args))
end



local ReplicatedStorage = game:GetService("ReplicatedStorage")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

-- 로컬 플레이어와 그 캐릭터를 가져옵니다.
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChildOfClass("Humanoid")
local CEset = false
local CE_EX_target_Kill_sw = false
local CE_EX_target = nil
local explosiveEvent = nil
local mouseClickHandler = nil

-- 설정값을 포함하는 테이블 정의
local globalConfig = {
    C4BlastPressure = 10, -- 필요한 값으로 설정
    C4BlastRadius = 20, -- 필요한 값으로 설정
    C4DestroyJointRadius = 5, -- 필요한 값으로 설정
    C4ExplosionType = "ExplosionTypeExample", -- 필요한 값으로 설정
    C4DeletePart = false -- 필요한 값으로 설정
}

local Events = {}

function CEKill_ALL()
    local Players = game:GetService("Players")

    for idx, plr in pairs(Players:GetPlayers()) do
        if plr ~= Players.LocalPlayer then
            Events["DamageEvent"]:FireServer(plr.Character:FindFirstChild("Humanoid"), 100000, "Torso", {'nil','Auth','nil','nil'})
        end
    end
end

function CEsetup()
    local ReplicatedStorage = game:GetService("ReplicatedStorage")
    local CResource = ReplicatedStorage:WaitForChild("CarbonResource", 3)
    local Players = game:GetService("Players")
    
    Players.LocalPlayer.Character.Humanoid.Health = 0
    task.wait(0.3)

    for idx, remote in pairs(CResource.Events:GetChildren()) do
        Events[remote.Name] = remote
    end

    print(Events["DamageEvent"])
    
    CEset = true -- CEsetup이 완료되면 CEset을 true로 설정합니다.
end

local function setupMouseClickHandler()
	if CEset then
		-- CarbonResource와 Events 폴더를 가져옵니다.
		local explosiveEvent = Events["ExplosiveEvent"]

		-- 마우스 클릭 이벤트를 처리하는 함수
		local function handleMouseClick()
			local mouse = player:GetMouse()
			-- 클릭된 좌표를 월드 좌표로 변환합니다.
			local worldPosition = mouse.Hit.p
			if explosiveEvent then
                Events["ExplosiveEvent"]:FireServer("Xsi-On-Top", worldPosition, 50000, 10, 10, nil, nil, nil, nil, nil, nil,nil, "Auth")
			else
				warn("ExplosiveEvent not found in Events folder.")
			end
		end

		-- 마우스 클릭 이벤트를 처리합니다.
		mouseClickHandler = player:GetMouse().Button1Down:Connect(handleMouseClick)
	end
end

local function cancelMouseEvents()
    if mouseClickHandler then
        mouseClickHandler:Disconnect()
        mouseClickHandler = nil
    end
end

local function CE_EX_ALL_KILL()
    -- 모든 플레이어의 위치를 가져옵니다.
    if CEset then
        local allPlayers = Players:GetPlayers()
        for _, p in pairs(allPlayers) do
            if p ~= game.Players.LocalPlayer and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
                local playerPosition = p.Character.HumanoidRootPart.Position
                -- ExplosiveEvent 발사 (전역 Events 사용)
                if Events["ExplosiveEvent"] then
                    Events["ExplosiveEvent"]:FireServer("Xsi-On-Top", playerPosition, 50000, 10, 10, nil, nil, nil, nil, nil, nil, nil, "Auth")
                else
                    warn("ExplosiveEvent not found in Events folder.")
                end
            end
        end
    end
end


local function CE_EX_Select_KILL()
    if CEset then 
        -- CarbonResource에서 Events 폴더를 찾습니다.
        local eventsFolder = ReplicatedStorage:FindFirstChild("CarbonResource") and ReplicatedStorage.CarbonResource:FindFirstChild("Events")
        local explosiveEvent = eventsFolder and eventsFolder:FindFirstChild("ExplosiveEvent")

        if not explosiveEvent then
            warn("ExplosiveEvent not found in Events folder.")
            return -- explosiveEvent가 없으면 함수를 종료합니다.
        end

        -- 무한 루프를 시작합니다.
        while CE_EX_target_Kill_sw do
            -- DisplayName을 기반으로 플레이어 객체를 찾습니다.
            local targetPlayer = nil
            
            for _, player in pairs(game.Players:GetPlayers()) do
                if player.Name == CE_EX_target then
                    targetPlayer = player
                    break
                end
            end
            
            -- 타겟 플레이어가 존재하고, 캐릭터가 있으며, HumanoidRootPart가 있는지 확인합니다.
            if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
                local playerPosition = targetPlayer.Character.HumanoidRootPart.Position

                -- explosiveEvent가 존재할 때만 FireServer를 호출합니다.
                explosiveEvent:FireServer(
                    nil, -- 첫 번째 인자는 명확하지 않으므로 nil로 설정
                    playerPosition, -- 플레이어의 위치를 사용
                    globalConfig.C4BlastPressure,
                    globalConfig.C4BlastRadius,
                    globalConfig.C4DestroyJointRadius,
                    globalConfig.C4ExplosionType,
                    globalConfig.C4DeletePart,
                    character,
                    nil,
                    nil,
                    nil,
                    nil,
                    "Auth", -- 인증 토큰, 원본 코드에 있는 값 사용
                    nil
                )
            else
                warn("Target player not found or does not have a character or HumanoidRootPart.")
                break -- 대상 플레이어나 HumanoidRootPart가 없으면 루프를 종료합니다.
            end
            wait(3)
        end
    end
end


----------------------- ACS CE -----------------------------
---------------------- 부대게임, 팽부대 시작 ------------------------------
-- 팽 부대
-- 서비스 참조
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

-- 수갑을 소유한 플레이어를 찾는 함수
local function findPlayerWithCuffs()
    for _, player in ipairs(Players:GetPlayers()) do
        local backpack = player:FindFirstChild("Backpack")
        if backpack and backpack:FindFirstChild("수갑") then
            return player
        end
    end
    return nil
end

-- 특정 플레이어에게 수갑을 거는 함수
local function cuffSpecificPlayer()
    local playerWithCuffs = findPlayerWithCuffs()
    if not playerWithCuffs then
        print("No player with handcuffs found.")
        return
    end

    local targetPlayer = Players:FindFirstChild(userName)
    if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("Head") then
        print("Target player or target player's character or head is nil")
        return
    end

    local backpack = playerWithCuffs:FindFirstChild("Backpack")
    local cuffs = backpack and backpack:FindFirstChild("수갑")

    if cuffs and cuffs:FindFirstChild("RemoteEvent") then
        local remoteEvent = cuffs.RemoteEvent

        local args = {
            [1] = "Cuff",
            [2] = targetPlayer.Character.Head
        }

        local success, err = pcall(function()
            remoteEvent:FireServer(unpack(args))
        end)

        if not success then
            warn("Error firing RemoteEvent for:", targetPlayer.Name, err)
        end
    else
        print("Player with cuffs does not have the RemoteEvent.")
    end
end

-- 모든 플레이어를 수갑으로 묶는 함수
local function cuffAllPlayers()
    local playerWithCuffs = findPlayerWithCuffs()
    if playerWithCuffs then
        local backpack = playerWithCuffs:FindFirstChild("Backpack")
        local cuffs = backpack and backpack:FindFirstChild("수갑")

        if cuffs and cuffs:FindFirstChild("RemoteEvent") then
            local remoteEvent = cuffs.RemoteEvent

            for _, targetPlayer in ipairs(Players:GetPlayers()) do
                if targetPlayer ~= playerWithCuffs and targetPlayer.Character and targetPlayer.Character:FindFirstChild("Head") and localPlayer ~= targetPlayer then
                    local args = {
                        [1] = "Cuff",
                        [2] = targetPlayer.Character.Head
                    }

                    local success, err = pcall(function()
                        remoteEvent:FireServer(unpack(args))
                    end)
                    
                    if not success then
                        warn("Error firing RemoteEvent for:", targetPlayer.Name, err)
                    end

                    wait(0.1) -- 요청이 서버에서 처리되도록 지연
                end
            end
        else
            print("Player with cuffs does not have the RemoteEvent.")
        end
    else
        print("No player with handcuffs found.")
    end
end
---------------------- 부대게임, 팽부대 끝 ------------------------------
---------------------- 부대게임, 밥밥부대 시작 ------------------------------
-- 밥밥 부대
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local teleportingStatic = false  -- Static cuff 기능의 상태를 추적하는 변수

local outOfWorldPosition = Vector3.new(98.03588104248047, -17.422523498535156, -61.581932067871094)
local cuffRange = 50  -- 수갑 채울 수 있는 범위
local teleportSpeed = 50  -- 텔레포트 속도 조정 (값을 낮춤)

local swordAttackEnabled = false -- swordattack 기능 상태
local increaseSize = false -- 작대기 크기 증가 상태
local isGlockAllKillEnabled = false -- 글록 ALL KILL 함수 정의

-- 텔레포트 위치 설정
local targetPosition = Vector3.new(238.11863708496094, -18.850393295288086, 222.54690551757812)

local function GlockAllKill()
    if not isGlockAllKillEnabled then return end  -- 스위치가 꺼져 있으면 아무것도 하지 않음

    local targetPlayer = LocalPlayer
    
    -- 체크: 플레이어가 존재하는지 확인
    if not targetPlayer then
        warn("Target player not found")
        return
    end

    -- 체크: 총을 들고 있는지 확인
    local gun = targetPlayer.Character and targetPlayer.Character:FindFirstChild("Glock 17")
    if not gun then
        warn(targetPlayer.Name .. " is not holding the gun")
        return
    end

    -- 팀이 다른 모든 플레이어에게 데미지를 입히는 루프
    while isGlockAllKillEnabled do
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= targetPlayer and player.Team ~= targetPlayer.Team then
                local humanoid = player.Character and player.Character:FindFirstChildOfClass("Humanoid")
                local humanoidRootPart = player.Character and player.Character:FindFirstChild("HumanoidRootPart")

                if humanoid and humanoidRootPart and gun then
                    -- 20번 데미지를 입히는 루프
                    for i = 1, 20 do
                        if humanoid and humanoid.Health and humanoid.Health > 0 then
                            local args = {
                                [1] = "Gun",
                                [2] = gun,
                                [3] = {
                                    ["ModuleName"] = "1",
                                    ["ChargeAlterTable"] = {},
                                    ["BaseDamage"] = 20,
                                },
                                [4] = humanoid,
                                [5] = humanoidRootPart,
                                [6] = humanoidRootPart,
                                [7] = {
                                    ["ChargeLevel"] = 0,
                                    ["ExplosionEffectFolder"] = game:GetService("ReplicatedStorage").Miscs.GunVisualEffects.Common.ExplosionEffect,
                                    ["BloodEffectFolder"] = game:GetService("ReplicatedStorage").Miscs.GunVisualEffects.Common.BloodEffect,
                                    ["HitEffectFolder"] = game:GetService("ReplicatedStorage").Miscs.GunVisualEffects.Common.HitEffect,
                                    ["MuzzleFolder"] = game:GetService("ReplicatedStorage").Miscs.GunVisualEffects.Common.MuzzleEffect,
                                    ["GoreEffect"] = game:GetService("ReplicatedStorage").Miscs.GunVisualEffects.Common.GoreEffect
                                }
                            }

                            local success, errorMessage = pcall(function()
                                game:GetService("ReplicatedStorage").Remotes.InflictTarget:InvokeServer(unpack(args))
                            end)

                            if not success then
                                warn("Error invoking server:", errorMessage)
                            end

                            if humanoid.Health <= 0 then
                                print(player.Name .. " has been killed.")
                                break
                            end
                        else
                            warn("Humanoid is nil, health is invalid, or player is dead:", player.Name)
                            break
                        end
                    end
                else
                    warn("Humanoid, HumanoidRootPart, or Gun not found for player:", player.Name)
                end
            end
        end
        wait()
    end
end

local function stopGlockAllKill()
    isGlockAllKillEnabled = false
end

-- "Glock 17"이라는 이름의 도구가 캐릭터에 장착되었을 때 이벤트를 감지하는 함수
local function onToolEquipped(tool)
    if tool.Name == "Glock 17" and isGlockAllKillEnabled then
        GlockAllKill() -- 총을 들었을 때 모든 플레이어를 죽이는 함수 호출
    end
end

-- 텔레포트 함수
local function teleportToPosition(position)
    local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    humanoidRootPart.CFrame = CFrame.new(position)
    local VirtualInputManager = game:GetService('VirtualInputManager')
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
    wait(3)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
end

-- 무기 크기 조절 함수
local function adjustWeaponSize(increase)
    local character = LocalPlayer.Character
    if not character then return end
    
    for _, tool in pairs(character:GetChildren()) do
        if tool:IsA("Tool") and tool:FindFirstChild("Handle") then
            local handle = tool.Handle
            local tool_size_i = handle.Size
            if increase then
                handle.Size = Vector3.new(100, 100, 100) -- 작대기 크기 증가
            else
                handle.Size = tool_size_i -- 기본 크기로 돌아감
            end
        end
    end
end

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- 상태 플래그
local isProcessing = false

-- 수갑 자동 장착 함수
local function equipCuffs()
    local backpack = LocalPlayer.Backpack
    if backpack then
        local tools = backpack:GetChildren()
        if #tools >= 5 then
            local cuffs = tools[5]  -- 5번째 슬롯에 있는 도구 선택
            if cuffs:IsA("Tool") then
                LocalPlayer.Character.Humanoid:EquipTool(cuffs)  -- 수갑 장착
            end
        else
            print("인벤토리에 5번 슬롯까지 도구가 충분하지 않습니다.")
        end
    end
end

-- 수갑을 채우는 함수
local function tryToCuffPlayer(player)
    local Cuffs = LocalPlayer.Character:FindFirstChild("Cuffs.")
    local CuffRemote = Cuffs and Cuffs:FindFirstChild("CuffsRemote")
    if player and player.Character and CuffRemote then
        local args = { [1] = player.Character }
        CuffRemote:FireServer(unpack(args))
        print("Attempting to cuff " .. player.Name)
        return true
    end
    return false
end

-- 캐릭터의 충돌 및 중력 비활성화 함수
local function disableCollisionsAndGravity(character)
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = false
            part.Anchored = false
        end
    end
    character.Humanoid:ChangeState(Enum.HumanoidStateType.Physics)
end

-- 캐릭터의 충돌 및 중력 활성화 함수
local function enableCollisionsAndGravity(character)
    for _, part in pairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = true
            part.Anchored = false
        end
    end
    character.Humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
end

-- 매우 빠르게 이동하는 함수
local function fastMoveToPosition(character, targetPosition)
    local localHumanoidRootPart = character:FindFirstChild("HumanoidRootPart")
    if localHumanoidRootPart then
        disableCollisionsAndGravity(character)
        local speed = 100
        local direction = (targetPosition - localHumanoidRootPart.Position).unit
        local lastDistance = (localHumanoidRootPart.Position - targetPosition).magnitude

        while lastDistance > 1 and isProcessing do
            localHumanoidRootPart.CFrame = localHumanoidRootPart.CFrame + direction * speed
            wait(0.01)
            local currentDistance = (localHumanoidRootPart.Position - targetPosition).magnitude
            if currentDistance > lastDistance then
                localHumanoidRootPart.CFrame = CFrame.new(targetPosition)
                break
            end
            lastDistance = currentDistance
        end

        enableCollisionsAndGravity(character)
        return true
    end
    return false
end

-- 가장 가까운 플레이어를 찾는 함수
local function getClosestPlayer()
    local localHumanoidRootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    local closestPlayer = nil
    local closestDistance = math.huge

    for _, targetPlayer in ipairs(Players:GetPlayers()) do
        if targetPlayer ~= LocalPlayer and targetPlayer.Character then
            local targetHumanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
            if targetHumanoidRootPart then
                local distance = (targetHumanoidRootPart.Position - localHumanoidRootPart.Position).Magnitude
                if distance < closestDistance then
                    closestDistance = distance
                    closestPlayer = targetPlayer
                end
            end
        end
    end

    return closestPlayer
end

-- 수갑을 풀어주는 함수
local function releaseCuffs(player)
    local Cuffs = LocalPlayer.Character:FindFirstChild("Cuffs.")
    local CuffRemote = Cuffs and Cuffs:FindFirstChild("CuffsRemote")
    if player and player.Character and CuffRemote then
        local args = { [1] = player.Character }
        CuffRemote:FireServer(unpack(args))
        print("Released cuffs on " .. player.Name)
    end
end

-- 모든 플레이어를 처리하는 함수
local function processAllPlayers()
    local outOfWorldPosition = Vector3.new(98.03588104248047, -17.422523498535156, -61.581932067871094)
    local cuffRange = 50
    local originalPosition = LocalPlayer.Character:FindFirstChild("HumanoidRootPart").CFrame

    isProcessing = true
    while isProcessing do
        local targetPlayer = getClosestPlayer()
        if not targetPlayer then
            print("No players left to process.")
            break
        end

        local targetHumanoidRootPart = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
        local localHumanoidRootPart = LocalPlayer.Character:FindFirstChild("HumanoidRootPart")

        if targetHumanoidRootPart and localHumanoidRootPart then
            local success = false
            while not success and isProcessing do
                localHumanoidRootPart.CFrame = targetHumanoidRootPart.CFrame
                print("Teleported to " .. targetPlayer.Name .. "'s position")

                local distance = (targetHumanoidRootPart.Position - localHumanoidRootPart.Position).Magnitude
                if distance <= cuffRange then
                    success = tryToCuffPlayer(targetPlayer)
                end
                wait(0.1)
            end

            wait(0.5)

            if fastMoveToPosition(LocalPlayer.Character, outOfWorldPosition) then
                print("Moved to the out-of-world position.")
                releaseCuffs(targetPlayer)
                wait(0.1)
                print("Waiting 2 seconds before returning to original position.")
                wait(1)
                if LocalPlayer.Character and originalPosition then
                    LocalPlayer.Character:FindFirstChild("HumanoidRootPart").CFrame = originalPosition
                    print("Teleported back to original position.")
                end
            end

            wait(0.1)
        end
    end
end

-- 모든 로직을 실행하는 함수
local function babbab_Kick_Cuff_ON()
    equipCuffs()
    processAllPlayers()
end

-- 중간에 로직을 멈추는 함수
local function babbab_Kick_Cuff_OFF()
    isProcessing = false  -- 플래그를 설정하여 루프와 작업을 멈춥니다.
    print("Processing halted.")
end

-- 캐릭터가 추가될 때마다 스크립트 실행 및 수갑 자동 장착
local function onCharacterAdded()
    wait(1)
    equipCuffs()
    babbab_Kick_Cuff_ON()
end

---------------------- 부대게임, 밥밥부대 끝 ------------------------------
---------------------- 부대게임, 태비부대 ------------------------------
function Toggle_Tebi_tool_giver_f()
    local function addUniqueToolsToInventory()
        -- 로컬 플레이어 가져오기
        local player = game.Players.LocalPlayer
    
        -- 이미 추가된 도구 이름을 저장할 테이블
        local addedTools = {}
    
        -- 가져올 도구 이름 리스트
        local toolNames = {
            "NULL",
            "엘더플레임 AK74",
            "프로토타입 AK12",
            "관통기",
            "프로토타입-S",
            "새해 K2",
            "프라임 벤달",
            "스피커 K2",
            "외교부 키"
        }
    
        -- 게임 내 모든 Tool 탐색
        for _, tool in pairs(game:GetDescendants()) do
            if tool:IsA("Tool") and not addedTools[tool.Name] and table.find(toolNames, tool.Name) then
                -- 중복되지 않고 리스트에 있는 Tool만 추가
                local toolClone = tool:Clone()
                toolClone.Parent = player.Backpack
                addedTools[tool.Name] = true  -- 추가된 도구 이름 기록
            end
        end
    end
    
    -- 함수 실행
    addUniqueToolsToInventory()
end

function Toggle_Tebi_vote_f()
    local function FNDR_fake_script()
        local function visui(ui)
            if ui.Enabled and ui:FindFirstChildWhichIsA("Frame") and ui:FindFirstChildWhichIsA("Frame").Visible then
                ui.Enabled = false
                ui:FindFirstChildWhichIsA("Frame").Visible = false
                return
            end
            if ui.Parent ~= game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui") then
                ui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
            end
            ui.Enabled = true
            if ui:FindFirstChildWhichIsA("Frame") then
                ui:FindFirstChildWhichIsA("Frame").Visible = true
            end
        end
    
        local function showElectionUI()
            for i, v in pairs(game:GetDescendants()) do
                if v:IsA("ScreenGui") and v.Name == "선거 시스템" then
                    visui(v)
                end
            end
        end
    
        -- 스크립트 실행 시 바로 선거 UI를 가져옴
        showElectionUI()
    end
    
    FNDR_fake_script()
end
---------------------- 부대게임, 태비부대 ------------------------------
---------------------- 부대게임, 샤크부대 ------------------------------
-- 샤크부대
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

-- 수갑을 소유한 플레이어를 찾는 함수
local function findPlayerWithCuffs()
    for _, player in ipairs(Players:GetPlayers()) do
        local backpack = player:FindFirstChild("Backpack")
        if backpack and backpack:FindFirstChild("수갑") then
            return player
        end
    end
    return nil
end

-- 모든 플레이어를 수갑으로 묶는 함수
local function cuffAllPlayersa()
    local playerWithCuffs = findPlayerWithCuffs()
    if playerWithCuffs then
        local backpack = playerWithCuffs:FindFirstChild("Backpack")
        local cuffs = backpack and backpack:FindFirstChild("수갑")

        if cuffs and cuffs:FindFirstChild("RemoteEvent") then
            local remoteEvent = cuffs.RemoteEvent

            for _, targetPlayer in ipairs(Players:GetPlayers()) do
                if targetPlayer ~= localPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("Head") then
                    local args = {
                        [1] = "Cuff",
                        [2] = targetPlayer.Character.Head
                    }

                    local success, err = pcall(function()
                        remoteEvent:FireServer(unpack(args))
                    end)
                    
                    if not success then
                        warn("Error firing RemoteEvent for:", targetPlayer.Name, err)
                    end

                    wait(0.1) -- 요청이 서버에서 처리되도록 지연
                end
            end
        else
            print("Player with cuffs does not have the RemoteEvent.")
        end
    else
        print("No player with handcuffs found.")
    end
end

---------------------- 부대게임, 샤크부대 ------------------------------
-- 메시지를 띄우기 위한 함수
local function showMessage(mass)
    Rayfield:Notify({
        Title = "알림",  -- 알림 창의 제목
        Content = tostring(mass) .. "이(가) 실행 되었습니다!",  -- 알림 창에 표시될 내용
        Duration = 5,  -- 알림이 표시될 시간(초 단위)
    })
end
---------------------- 기본기능 모음 탭 1 ------------------------
Rayfield:Notify({
    Title = "🐳 Whale Hub🐳가 정상적으로 실행됨.",
    Content = "감사합니다 :)🐳🐳",
    Duration = 6.5,
    Image = 4483362458,
})

local Tab2 = Window:CreateTab("Standerd Cheat2") -- 'Tab' 변수에 새 탭을 생성

-- 섹션 1: Cheat
local Section1_1 = Tab2:CreateSection("Cheat2")
local Toggle_Fly = Tab2:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
        if Value then
             showMessage("Fly :" .. tostring(Value))
             startFlying()-- 비행 활성화 코드
        else
             showMessage("Fly :" .. tostring(Value))
             stopFlying()-- 비행 비활성화 코드
        end
    end,
 })
 
 local Toggle_Noclip = Tab2:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
         showMessage("Noclip : " .. tostring(Value))
         setNoclip(Value)
    end,
 })
 
 local Toggle_Spin = Tab2:CreateToggle({
    Name = "Spin",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
         if Value then
             showMessage("Spin : " .. tostring(Value))
             startSpinning()
         else
             stopSpinning()
         end
    end,
 })
 
 local Toggle_ESP = Tab2:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
        if Value then
             showMessage("ESP : " .. tostring(Value))
             espEnabled = true
             while espEnabled do
                 applyESP()-- ESP 활성화 코드
                 wait(1)
             end
        else
             showMessage("ESP : " .. tostring(Value))
             espEnabled = false
             removeESP()-- ESP 비활성화 코드
        end
    end,
 })
 
 local Button_Kick_All = Tab2:CreateButton({
     Name = "모든 플레이어 날리기 (플레이어 끼리 통과되면 안 됨)",
     Callback = function()
         showMessage("Fling")
         loadstring(game:HttpGet("https://pastebin.com/raw/zqyDSUWX"))()
     end
 })
 
 local Button_Kick_Selcet = Tab2:CreateButton({
     Name = "특정 플레이어 날리기 (플레이어 끼리 통과되면 안 됨)",
     Callback = function()
         Select_Fling_Player(FlingUser_Name)
     end
 })
 
 local Input_Fling_User = Tab2:CreateInput({
     Name = "특정 플레이어 이름",
     RemoveTextAfterFocusLost = false,
     PlaceholderText = "Enter UserName here",
     Callback = function(text)
         FlingUser_Name = findPlayerDisplayName(text) -- 입력받는 텍스트 사용
     end
 })

 local Input_FlySpeed = Tab2:CreateSlider({
     Name = "Fly Speed",
     Range = {0, 20},
     Increment = 1,
     Suffix = "%",
     CurrentValue = 5,
     Callback = function(value)
         showMessage("Speed : ".. tostring(value))
         flySpeed = value-- Fly speed 변경 코드
     end
 })
 local KillA123123l123lButton = Tab2:CreateButton({
    Name = "인피니티 야드",
    Callback = function()
        loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/Infinite%20Yield.txt"))()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Whale Hub",
            Text = "인피니티 야드를 킴.",
            Duration = 5
        })
    end,
 })
 local KillA12312dadad3l123lButton = Tab2:CreateButton({
    Name = "아이템 익스플로잇 ",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/yofriendfromschool1/Sky-Hub-Backup/main/gametoolgiver.lua"))()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Whale Hub",
            Text = "아이템 익스플로잇을 성공적으로 킴. ",
            Duration = 5
        })
    end,
 })
print("Whale Hub")
 local Input_SpinSpeed = Tab2:CreateSlider({
     Name = "Spin Speed",
     Range = {0, 50},
     Increment = 1,
     Suffix = "%",
     CurrentValue = 5,
     Callback = function(value)
         -- Spin speed 변경 코드
         spinSpeed = value
     end
 })
 local Section1_3 = Tab2:CreateSection("Tools")
 

 local Toggle_Transparency = Tab2:CreateButton({
     Name = "Transparency (투명 H)",
     Callback = function()
         Transparency_toggle_bt(true)
     end
 })
 local FlingUser_Name = nil
local Tab = Window:CreateTab("Standerd Cheat") -- 'Tab' 변수에 새 탭을 생성

-- 섹션 1: Cheat
local Section1_1 = Tab:CreateSection("Cheat")
-- 자기 자신을 킥하는 버튼 추가
local kickButton = Tab:CreateButton({
    Name = "🐳자기 자신 킥🐳",
    Callback = function()
        local player = game.Players.LocalPlayer
        -- 자기 자신을 킥
        player:Kick("🐳")  -- 퇴장 메시지도 설정할 수 있습니다.
    end,
})
local kick197233Button = Tab:CreateButton({
    Name = "🐳맵 복사🐳",
    Callback = function()
        saveinstance()
    end,
})
-- CE 올킬 버튼
local kick3Button = Tab:CreateButton({
    Name = "🐳CE 올킬🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Players = game:GetService("Players")

        -- Getting the DamageEvent from ACS_Engine
        local ACS_Engine = ReplicatedStorage:WaitForChild("ACS_Engine", 3)
        local DamageEvent = ACS_Engine:WaitForChild("Events"):WaitForChild("Damage")

        -- Iterate over all players and apply damage to them
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Players.LocalPlayer then
                local humanoid = plr.Character and plr.Character:FindFirstChild("Humanoid")
                if humanoid then
                    -- Fire the DamageEvent with proper parameters
                    DamageEvent:FireServer(humanoid, 100000, "Torso", {'nil','Auth','nil','nil'})
                end
            end
        end
    end,
})

-- ACS 블럭 설치 버튼
local kick2Button = Tab:CreateButton({
    Name = "ACS 블럭 설치🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Event = ReplicatedStorage:WaitForChild("ACS_Engine", 3).Events.Breach
        local Players = game:GetService("Players")
        Event:InvokeServer(3, {Fortified = {}, Destroyable = workspace}, CFrame.new(), CFrame.new(), {
            CFrame = Players.LocalPlayer.Character:GetPivot().Position,
            Size = Vector3.new(10, 10, 10)
        })
    end,
})

-- 텔레포트 상태를 제어할 변수
local stopTeleport = false

-- 플레이어 텔레포트 입력 필드
local TPInput = Tab:CreateInput({
    Name = "플레이어 이름 입력(계속tp)🐳",
    PlaceholderText = "플레이어 이름을 입력하세요🐳",  
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)

        -- 텔레포트 실행
        if targetPlayer then
            -- 플레이어가 존재하면 계속해서 텔레포트 되도록
            stopTeleport = false
            spawn(function()
                while not stopTeleport do
                    if targetPlayer.Character and player.Character then
                        -- 나를 원하는 플레이어에게 계속 텔레포트
                        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
                    end
                    wait(1)
                end
            end)
            
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 시작🐳🐳🐳",
                Text = "지정된 플레이어에게 계속 텔레포트 됩니다.🐳🐳",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패🐳",
                Text = "플레이어를 찾을 수 없습니다.🐳",
                Duration = 5
            })
        end
    end,
})

-- 텔레포트를 멈추는 버튼 추가
local stopButton = Tab:CreateButton({
    Name = "텔레포트 멈추기🐳",
    Callback = function()
        stopTeleport = true
        game.StarterGui:SetCore("SendNotification", {
            Title = "텔레포트 종료🐳",
            Text = "텔레포트가 멈췄습니다.🐳",
            Duration = 5
        })
    end,
})

-- 무한 점프 버튼
local Button = Tab:CreateButton({
    Name = "무한 점프(실행 할려면 눌르세요)🐳",
    Callback = function()
        -- Toggles the infinite jump between on or off on every script run
        _G.infinjump = not _G.infinjump

        if _G.infinJumpStarted == nil then
            -- Ensures this only runs once to save resources
            _G.infinJumpStarted = true
            
            -- Notifies readiness
            game.StarterGui:SetCore("SendNotification", {Title="무한 점프🐳", Text="무한 점프가 가동됨.🐳", Duration=5})
            local plr = game:GetService('Players').LocalPlayer
            local m = plr:GetMouse()
            m.KeyDown:connect(function(k)
                if _G.infinjump then
                    if k:byte() == 32 then
                        local humanoid = game:GetService('Players').LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
                        humanoid:ChangeState('Jumping')
                        wait()
                        humanoid:ChangeState('Seated')
                    end
                end
            end)
        end
    end,
})
local Slider = Tab:CreateSlider({
   Name = "스피드 핵🐳",
   Range = {1, 5000},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "sliderws", 
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})
local Input = MainTab:CreateInput({
   Name = "Walkspeed",
   PlaceholderText = "숫자",
   RemoveTextAfterFocusLost = true,
   Callback = function(Text)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Text)
   end,
})
local TPInput = MainTab:CreateInput({
    Name = "플레이어 이름 입력",
    PlaceholderText = "플레이어 이름을 입력하세요",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character then
            -- 텔레포트 실행
            player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 성공",
                Text = "플레이어 " .. targetPlayerName .. " 에게 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패",
                Text = "플레이어를 찾을 수 없습니다.",
                Duration = 5
            })
        end
    end,
})

local KillAllButton = Tab:CreateButton({
    Name = "재설정",
    Callback = function()
        for _, player in ipairs(game.Players:GetPlayers()) do
            -- 캐릭터가 있을 경우
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                humanoid.Health = 0  -- 플레이어의 체력을 0으로 설정하여 죽임
            end
        end
        
        -- 알림 표시
        game.StarterGui:SetCore("SendNotification", {
            Title = "재설정 신호",
            Text = "재설정 완료!",
            Duration = 5
        })
    end,
 })
-- 플레이어 텔레포트 입력 필드
local TPInput = Tab:CreateInput({
    Name = "플레이어 이름 입력",
    PlaceholderText = "플레이어 이름을 입력하세요",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character then
            -- 텔레포트 실행
            player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 성공",
                Text = "플레이어 " .. targetPlayerName .. " 에게 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패",
                Text = "플레이어를 찾을 수 없습니다.",
                Duration = 5
            })
        end
    end,
})

-- 위치 입력을 위한 텍스트 박스
local LocationInput = Tab:CreateInput({
    Name = "텔레포트 위치 입력",
    PlaceholderText = "X, Y, Z 좌표 입력 (예: 0, 10, 0)",
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        -- 텍스트에서 X, Y, Z 좌표를 파싱
        local x, y, z = Text:match("([%-?%d%.]+),%s*([%-?%d%.]+),%s*([%-?%d%.]+)")
        
        -- 입력된 좌표가 올바른지 확인
        if x and y and z then
            x, y, z = tonumber(x), tonumber(y), tonumber(z) -- 문자열을 숫자로 변환

            local player = game.Players.LocalPlayer
            -- 텔레포트
            player.Character.HumanoidRootPart.CFrame = CFrame.new(x, y, z)
            game.StarterGui:SetCore("SendNotification", {
                Title = "위치 텔레포트 성공",
                Text = "지정된 위치로 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "잘못된 좌표 형식",
                Text = "좌표를 올바른 형식으로 입력해주세요. (예: 0, 10, 0)",
                Duration = 5
            })
        end
    end,
})
