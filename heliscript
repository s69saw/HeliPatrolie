
--HELIPATROLIE	--	HeliPatrolie
local HeliPatrolieID = 0
local HeliPatrolieTimer = 0
local HeliPatrolieReSpawntimer = 0
--	Status	--
local HeliPatrolieOnline = false
local HeliPatroliePly2On = false
local HeliStatus = -1
-----------------------
--	Helicopter --
local HelikopterV = nil
-- Peds --
local HeliGuardPedDriver = nil
local HeliGuardPed1 = nil
local HeliGuardPed2 = nil
local HeliGuardPed3 = nil

local HeliSpectatorMode = false

--###############################################################################################################################
--###############################################################################################################################

RegisterNetEvent('NPC:ClientUpdateHeliPatroliePed')
AddEventHandler('NPC:ClientUpdateHeliPatroliePed', function(HeliOn,UserID)
	if HeliOn then
		if UserID == 0 then
			HeliPatroliePly2On = true
		else
			HeliPatrolieID = UserID
			SetzeHeliGardStatusON()
			HeliGuardSpawn()
			Citizen.Trace("ClientUpdateHeliPatroliePed  --  NEU ERSTELLT  --  ID: ("..HeliPatrolieID..")--Debug--  \n")
		end	
	end
end)

--############################################################################################################
--############################################################################################################
RegisterNetEvent('NPC:ClientHelikopterNeuStart')
AddEventHandler('NPC:ClientHelikopterNeuStart', function()
	HeliPatrolieID = 0
	HeliPatrolieTimer = 0
	HeliPatrolieReSpawntimer = 0
	--	Status	--
	HeliPatrolieOnline = false
	HeliStatus = -1
	-----------------------
	--	Helicopter --
	HelikopterV = nil
	-- Peds --
	HeliGuardPedDriver = nil
	HeliGuardPed1 = nil
	HeliGuardPed2 = nil
	HeliGuardPed3 = nil
	-----------------------
	Citizen.Trace("ClientHelikopterNeuStart --HeliPatrolie auf Zeitschleife gesetzt! --Debug--  \n")
	SetTimeout(math.random(300000,1800000), SetzeHeliGardOnlline) 	--	5-30min	--		Citizen.Wait(math.random(60000,300000))	
	--SetzeHeliGardOnlline()	
end)
--###############################################################################################################################
--###############################################################################################################################

AddEventHandler("playerSpawned", function(spawn)
--	SetTimeout(300000, SetzeHeliGardOnlline) 
    SetTimeout(math.random(300000,1800000), SetzeHeliGardOnlline) 	--	5-30min	--		
end)
--################################################################################################################
function SetzeHeliGardOnlline()
	SpielerID = GetPlayerServerId(PlayerId())
	TriggerServerEvent('NPC:HeliPatrolieOnlineSV', SpielerID)	--	starter
	--Citizen.Trace("SetzeHeliGardOnlline --HeliPatrolie--Online-- --Debug--  \n")
	
end
--################################################################################################################
--################################################################################################################
function SetzeHeliGardStatusON()
	if HeliStatus == -1 then
		HeliStatus = 99
	end
end
--###############################################################################################################

function DeleteAllHeliGuards(wert)
	if wert == 0 then	--	mission beendet	--	
		SetEntityAsMissionEntity(HelikopterV, false, false)
		SetEntityAsNoLongerNeeded(HelikopterV)
		SetEntityAsNoLongerNeeded(HeliguardPeddriver)
		SetEntityAsNoLongerNeeded(HeliGuardPed1)
		SetEntityAsNoLongerNeeded(HeliGuardPed2)
		SetEntityAsNoLongerNeeded(HeliGuardPed3)
		Wait(60000)
	elseif wert == 1 or wert == 2 then	--	heliflieger tot	-- oder	--	Heli kaputt	--
		SetEntityAsNoLongerNeeded(HelikopterV)
		SetEntityAsNoLongerNeeded(HeliguardPeddriver)
		SetEntityAsNoLongerNeeded(HeliGuardPed1)
		SetEntityAsNoLongerNeeded(HeliGuardPed2)
		SetEntityAsNoLongerNeeded(HeliGuardPed3)
		Wait(60000)
		DeletePed(Citizen.PointerValueIntInitialized(HeliguardPeddriver))
		DeletePed(Citizen.PointerValueIntInitialized(HeliGuardPed1))
		DeletePed(Citizen.PointerValueIntInitialized(HeliGuardPed2))
		DeletePed(Citizen.PointerValueIntInitialized(HeliGuardPed3))
		DeletePed(HeliguardPeddriver)
		DeletePed(HeliGuardPed1)
		DeletePed(HeliGuardPed2)
		DeletePed(HeliGuardPed3)
	elseif wert == 3 then	--	ablauf des respawntimer
		TriggerServerEvent('NPC:DelteHeliPatroliePed')	--	server wert auf  0
	end
	HeliStatus = 100
	HeliPatrolieTimer = 0
	HeliPatrolieOnline = false
	HeliPatroliePly2On = false
	HeliPatrolieReSpawntimer = math.random(300000, 900000)	--5min - 15min
	Citizen.Trace(">>-->   [Studio69-Server] HeliGuards - Parameter alle entfernt!\n")
end	
--##############################################--##############################################	
--##############################################--##############################################
RegisterCommand('helicam', function(source, args)
	if IsPedInAnyVehicle(GetPlayerPed(-1), false) then
		drawNotification("~r~Not in a vehicle.")
		return
	else
		HeliSpectatorMode = not HeliSpectatorMode
		if HeliSpectatorMode == true then
			if DoesEntityExist(HeliguardPeddriver) then
				StartScreenEffect("SwitchShortTrevorMid", 800, false)
				PlaySound(-1, "slow", "SHORT_PLAYER_SWITCH_SOUND_SET", 0, 0, 1)
				FreezeEntityPosition(GetPlayerPed(-1),  true)
				NetworkSetInSpectatorMode(1, HeliguardPeddriver)
			--	NetworkSetOverrideSpectatorMode(true)
				NetworkSetActivitySpectator(HeliguardPeddriver)
				SetFollowPedCamViewMode(3)	-- verschieden stellungen
				Wait(60000)
				StartScreenEffect("SwitchShortTrevorMid", 800, false)
				PlaySound(-1, "slow", "SHORT_PLAYER_SWITCH_SOUND_SET", 0, 0, 1)
				NetworkSetInSpectatorMode(1, GetPlayerPed(-1))
				NetworkSetInSpectatorMode(0, GetPlayerPed(-1))
			--	NetworkSetOverrideSpectatorMode(false)
				HeliSpectatorMode = false
				Wait(500)
				SetFollowPedCamViewMode(0)	-- verschieden stellungen
				FreezeEntityPosition(GetPlayerPed(-1),  false)
			else
				drawNotification("~o~HeliguardPeddriver nicht Online!")
				PlaySoundFrontend(-1, "Enter_Capture_Zone", "DLC_Apartments_Drop_Zone_Sounds", 1)
			end
		end
	end
end)
--###############################################################################################################################
--###############################################################################################################################
function HeliGuardSpawn()
	if HeliStatus == 99 then
		if not HeliPatrolieOnline then
			--Citizen.Trace("HeliGuards werdern erstellt!\n")
			local string = "[Studio69-Server] HeliGuads werdern erstellt !"
			TriggerServerEvent("NPC:serverlog", string)	--server--

			local HelikopterModel = GetHashKey("Valkyrie")	--	BUZZARD	Annihilator
			RequestModel(HelikopterModel)
			while not HasModelLoaded(HelikopterModel) do
				Citizen.Wait(0)
			end

			local EscortPedModel = GetHashKey("MP_S_M_Armoured_01")	--	S_M_M_Armoured_01	S_M_M_Armoured_02
			RequestModel(EscortPedModel)
			while not HasModelLoaded(EscortPedModel) do
				Citizen.Wait(0)
			end
			
			HelikopterV = CreateVehicle(HelikopterModel, -74.69,-818.87,326.18,295.72, true, false)
			VehToNet(HelikopterV)
			N_0x06faacd625d80caa(HelikopterV)
			SetVehicleOnGroundProperly(HelikopterV)
			SetEntityInvincible(HelikopterV,true)
			HeliguardPeddriver = CreatePedInsideVehicle(HelikopterV, 5,  EscortPedModel, -1, true, true)
			SetzeHeliGard(HeliguardPeddriver)--fahrer
			HeliGuardPed1 = CreatePedInsideVehicle(HelikopterV, 27,  EscortPedModel, 0, true, true)
			SetzeHeliGard(HeliGuardPed1)
			HeliGuardPed2 = CreatePedInsideVehicle(HelikopterV, 27,  EscortPedModel, 1, true, true)
			SetzeHeliGard(HeliGuardPed2)
			HeliGuardPed3 = CreatePedInsideVehicle(HelikopterV, 27,  EscortPedModel, 2, true, true)
			SetzeHeliGard(HeliGuardPed3)
			SetModelAsNoLongerNeeded(HelikopterModel)
			SetModelAsNoLongerNeeded(EscortPedModel)
			HeliStatus = 22
			--Citizen.Trace("#[Studio69-Server] HeliGuads online und auf Patrolie: "..HelikopterV)
			HeliPatrolieOnline = true
			SetTimeout(math.random(10000,99000), WarteschleifeHeliGard)
		end
	end	
end

--###############################################################################################################################
--##############################################-WarteschleifeHeliGard-##############################################
--###############################################################################################################################
function WarteschleifeHeliGard()
	if HeliPatrolieHeliStaus == 22 then
		Wait(1000)
		SetEntityAsMissionEntity(HelikopterV, true, true)
		if IsPedInAnyVehicle(HeliguardPeddriver, true) then
			if	IsPedInAnyVehicle(HeliGuardPed1, true) and IsPedInAnyVehicle(HeliGuardPed2, true) and IsPedInAnyVehicle(HeliGuardPed3, true) then
				SetEntityInvincible(HelikopterV,false)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 692.19,-2943.45,50.75, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
				TriggerServerEvent("SyncAd", 'copreport', "~y~Kampf-Helikopter Wachschutz gestartet!")
				PlaySound(-1, "Oneshot_Final", "MP_MISSION_COUNTDOWN_SOUNDSET", 0, 0, 1)
				HeliPatrolieTimer = 600
				HeliPatrolieHeliStaus = 0
			else
				TaskWarpPedIntoVehicle(HeliGuardPed1, HelikopterV, 0)
				TaskWarpPedIntoVehicle(HeliGuardPed2, HelikopterV, 1)
				TaskWarpPedIntoVehicle(HeliGuardPed3, HelikopterV, 2)
				SetTimeout(1000, WarteschleifeHeliGard) -- WarteschleifeHeliGard()
			end
		else
			TaskWarpPedIntoVehicle(HeliguardPeddriver, HelikopterV, -1)
			SetTimeout(1000, WarteschleifeHeliGard) 
		end
	end
end
--###############################################################################################################################
--##############################################-HeliGuardsInAktion-##############################################
--###############################################################################################################################

function HeliGuardsInAktion()
	if HeliPatrolieOnline then
		if HeliStatus == 0 then
			if IsPedNearCoords(HelikopterV,692.19,-2943.45,50.75, 20 ) then 
				HeliStatus = 1
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -50.78,-2513.99,74.48, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end	
		elseif HeliStatus == 1 then
			if IsPedNearCoords(HelikopterV,-50.78,-2513.99,74.48, 20 ) then 
				HeliStatus = 2
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 64.5,-1904.81,52.94, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 2 then
			if IsPedNearCoords(HelikopterV,64.5,-1904.81,52.94, 20 ) then 
				HeliStatus = 3
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 203.65,-919.21,79.96, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 3 then
			if IsPedNearCoords(HelikopterV,203.65,-919.21,79.96, 20 ) then 
				HeliStatus = 4
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 191.98,-251.15,116.88, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 4 then
			if IsPedNearCoords(HelikopterV,191.98,-251.15,116.88, 20 ) then 
				HeliStatus = 5
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -1100.42,-289.27,99.16, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 5 then
			if IsPedNearCoords(HelikopterV,-1100.42,-289.27,99.16, 20 ) then 
				HeliStatus = 6
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -1884.0,-776.36,45.3, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 6 then
			if IsPedNearCoords(HelikopterV,-1884.0,-776.36,45.3, 20 ) then 
				HeliStatus = 7
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -1260.31,-1827.46,34.06, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 7 then
			if IsPedNearCoords(HelikopterV,-1260.31,-1827.46,34.06, 20 ) then 
				HeliStatus = 8
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -94.35,-466.71,70.56, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 8 then
			if IsPedNearCoords(HelikopterV,-94.35,-466.71,70.56, 20 ) then 
				HeliStatus = 9
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 2574.99,-285.86,122.99, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 9 then
			if IsPedNearCoords(HelikopterV,2574.99,-285.86,122.99, 20 ) then 
				HeliStatus = 10
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 1918.6,2505.96,81.97, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 10 then
			if IsPedNearCoords(HelikopterV,1918.6,2505.96,81.97, 20 ) then 
				HeliStatus = 11
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 2464.76,5704.54,135.82, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 11 then
			if IsPedNearCoords(HelikopterV,2464.76,5704.54,135.82, 20 ) then 
				HeliStatus = 12
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -430.03,6025.29,57.5, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 12 then
			if IsPedNearCoords(HelikopterV,-430.03,6025.29,57.5, 20 ) then 
				HeliStatus = 13
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -1345.38,5288.01,120.57, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 13 then
			if IsPedNearCoords(HelikopterV,-1345.38,5288.01,120.57, 20 ) then 
				HeliStatus = 14
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -2295.25,4280.47,75.1, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 14 then
			if IsPedNearCoords(HelikopterV,-2295.25,4280.47,75.1, 20 ) then 
				HeliStatus = 15
				HeliPatrolieTimer = 300
				Wait(100)
				TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, -1857.75,2773.71,60.99, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
			end
		elseif HeliStatus == 15 then
			if IsPedNearCoords(HelikopterV,-1857.75,2773.71,60.99, 20 ) then 
				--Citizen.Trace(">>-->   Helikopter und Guards angekommen!\n")
				HeliStatus = 16
				HeliPatrolieTimer = 300
				Wait(30000)
				DeleteAllHeliGuards(0)
			end
		end
	end
end


--###############################################################################################################################
--###############################################################################################################################
--			
Citizen.CreateThread(function()
	while true do
		Citizen.Wait(1000)
		-------------------------------------------------------------------
		if HeliPatrolieReSpawntimer > 1 then
			HeliPatrolieReSpawntimer = HeliPatrolieReSpawntimer - 1
		elseif HeliPatrolieReSpawntimer == 1 then
			HeliStatus = -1
			HeliPatrolieReSpawntimer = 0
			DeleteAllHeliGuards(3)
		end
		-------------------------------------------------------------------
		if HeliPatrolieTimer > 1 then
			HeliPatrolieTimer = HeliPatrolieTimer - 1
		elseif HeliPatrolieTimer == 1 then
			HeliPatrolieTimer = 0
			HeliStatus = 0
			TaskVehicleDriveToCoord(HeliguardPeddriver, HelikopterV, 692.19,-2943.45,50.75, 40.0, false, GetEntityModel(HelikopterV), 786603, 1.0, true)
		end
		-------------------------------------------------------------------
		if HeliPatrolieOnline then
			if HeliStatus >= 0 and HeliStatus <= 98 then
				if IsEntityDead(HeliguardPeddriver) then
					--Citizen.Trace(">INFO<: HeliGuardPedDriver ist gestorben")
					local string = ">INFO<: HeliGuardPedDriver ist gestorben "
					TriggerServerEvent("NPC:serverlog", string)	--server--
					TriggerServerEvent("SyncAd", 'copreport', "~o~Kampf-HeliGuardPedDriver ist gestorben!")
					PlaySound(-1, "Oneshot_Final", "MP_MISSION_COUNTDOWN_SOUNDSET", 0, 0, 1)
					DeleteAllHeliGuards(1)
				end
				local damage = GetEntityHealth(HelikopterV)
				if damage <= 1 then
					--Citizen.Trace(">INFO<: Helikopter ist defekt!")
					local string = ">INFO<: Helikopter ist defekt! "
					TriggerServerEvent("NPC:serverlog", string)	--server--
					TriggerServerEvent("SyncAd", 'copreport', "~o~Kampf-Helikopter ist Defekt!")
					PlaySound(-1, "Oneshot_Final", "MP_MISSION_COUNTDOWN_SOUNDSET", 0, 0, 1)
					DeleteAllHeliGuards(2)
				end
				HeliGuardsInAktion()	--	SetTimeout(math.random(30000,90000), HeliGuardsInAktion)
			end
		end
		-------------------------------------------------------------------
	end	
end)
