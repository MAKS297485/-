---@diagnostic disable: undefined-global, need-check-nil, lowercase-global, cast-local-type, unused-local

script_name("Vanguard Helper")
script_description('This is a Lua script helper for Rodina RP players who work in the MVD')
script_author("Milky")
script_version("1.7 alpha")

require('lib.moonloader')
require('encoding').default = 'CP1251'
local u8 = require('encoding').UTF8
local ffi = require('ffi')
local sizeX, sizeY = getScreenResolution()
local encoding = require('encoding')
encoding.default = 'CP1251'
u8 = encoding.UTF8


print('[Vanguard Helper] Скрипт загружен. Версия: ' .. thisScript().version)
-------------------------------------------- JSON SETTINGS ---------------------------------------------
local configDirectory = getWorkingDirectory():gsub('\\','/') .. "/Vanguard Helper"
local path_helper = getWorkingDirectory():gsub('\\','/') .. "/Vanguard Helper.lua"
local path_settings = configDirectory .. "/Settings.json"
local settings = {}
local default_settings = {
	general = {
		version = thisScript().version,
		accent_enable = true,
		auto_mask = true,
		rp_chat = true,
        rp_gun = true,
		auto_doklad_patrool = false,
		auto_doklad_damage = false,
		auto_doklad_arrest = true,
		auto_change_code_siren = false,
		auto_update_wanteds = false,
		auto_find_wanteds = false,
		auto_update_members = false,
		auto_notify_payday = true,
		auto_notify_port = true,
		auto_accept_docs = false,
		auto_uval = false,
		auto_time = true,
		auto_clicker_situation = false,
		auto_documentation = false,
		moonmonet_theme_enable = true,
		moonmonet_theme_color = 40703,
		mobile_fastmenu_button = true,
		mobile_stop_button = true,
		mobile_meg_button = true,
		use_binds = true,
		use_info_menu = true,
		use_taser_menu = true,
		bind_mainmenu = '[113]',
		bind_fastmenu = '[69]',
		bind_leader_fastmenu = '[71]',
		bind_command_stop = '[123]',
		custom_dpi = 1.0,
		autofind_dpi = false,
	},
	info_display = {
		city = true,
		area = true,
		square = true,
		time = true,
		mask = true
	},
	player_info = {
		name_surname = '',
		accent = '[Иностранный акцент]:',
		fraction = 'Неизвестно',
		fraction_tag = 'Неизвестно',
		fraction_rank = 'Неизвестно',
		fraction_rank_number = 0,
		sex = 'Неизвестно',
	},
	deportament = {
		dep_fm = '-',
		dep_tag1 = '',
		dep_tag2 = '[Всем]',
		dep_tags = {
			"[Всем]",
			"[Похитители]",
			"[Терористы]",
			"[Диспетчер]",
			'skip',
			"[МЮ]",
			"[Мин.Юст.]",
			"[ЛСПД]",
			"[СФПД]",
			"[ЛВПД]",
			"[РКШД]",
			"[СВАТ]",
			"[ФСБ]",
			'skip',
			"[МО]",
			"[Мин.Обороны]",
			"[ЛСа]",
			"[СФа]",
			"[ТСР]",
			'skip',
			"[МЗ]",
			"[МЗП]",
			"[Мин.Здрав.]",
			"[ЛСМЦ]",
			"[СФМЦ]",
			"[ЛВМЦ]",
			"[ДМЦ]",
			"[ПД]",
			'skip',
			"[ЦА]",
			"[ЦЛ]",
			"[СК]",
			"[Пра-во]",
			"[Губернатор]",
			"[Прокурор]",
			'skip',
			"[СМИ]",
			"[СМИ ЛС]",
			"[СМИ СФ]",
			"[СМИ ЛВ]",
		},
		dep_tags_en = {
			"[ALL]",
			'skip',
			"[MJ]",
			"[Min.Just.]",
			"[LSPD]",
			"[SFPD]",
			"[LVPD]",
			"[RCSD]",
			"[SWAT]",
			"[FBI]",
			'skip',
			"[MD]",
			"[Mid.Def.]",
			"[LSa]",
			"[SFa]",
			"[MSP]",
			'skip',
			"[MH]",
			"[MHF]",
			"[Min.Healt]",
			"[LSMC]",
			"[SFMC]",
			"[LVMC]",
			"[JMC]",
			"[FD]",
			'skip',
			"[GOV]",
			"[Prosecutor]",
			"[LC]",
			"[INS]",
			'skip',
			"[CNN]",
			"[CNN LS]",
			"[CNN LV]",
			"[CNN SF]",
		},
		dep_tags_custom = {},
		dep_fms = {
			'-',
			'- з.к. -',
		},
		dep_fms_custom = {},
	},
	windows_pos = {
		megafon = {x = sizeX / 8.5, y = sizeY / 2.1},
		info_menu = {x = sizeX * 0.03, y = sizeY * 0.53},
		patrool_menu = {x = sizeX / 8, y = sizeY - sizeY/10},
		wanteds_menu = {x = sizeX / 1.2, y = sizeY / 2},
		mobile_fastmenu_button = {x = sizeX / 8.5, y = sizeY / 2.3},
		taser = {x = sizeX / 4.2, y = sizeY / 2.1},
	}
}

function load_settings()
    if not doesDirectoryExist(configDirectory) then
        createDirectory(configDirectory)
    end
    if not doesFileExist(path_settings) then
        settings = default_settings
		print('[Vanguard Helper] Файл с настройками не найден, использую стандартные настройки!')
    else
        local file = io.open(path_settings, 'r')
        if file then
            local contents = file:read('*a')
            file:close()
			if #contents == 0 then
				settings = default_settings
				print('[Vanguard Helper] Не удалось открыть файл с настройками, использую стандартные настройки!')
			else
				local result, loaded = pcall(decodeJson, contents)
				if result then
					settings = loaded
					for category, data in pairs(default_settings) do
						if settings[category] == nil then
							settings[category] = data
						elseif type(data) == 'table' then
							for key, value in pairs(data) do
								if settings[category][key] == nil then
									settings[category][key] = value
								end
							end
						end
					end
					if settings.general.version ~= thisScript().version then
						print('[Vanguard Helper] Новая версия, сброс настроек!')
						settings = default_settings
						save_settings()
						reload_script = true
					else
						print('[Vanguard Helper] Настройки успешно загружены!')
					end
				else
					print('[Vanguard Helper] Не удалось открыть файл с настройками, использую стандартные настройки!')
				end
			end
        else
            settings = default_settings
			print('[Vanguard Helper] Не удалось открыть файл с настройками, использую стандартные настройки!')
        end
    end
end
function save_settings()
    local file, errstr = io.open(path_settings, 'w')
    if file then
        local result, encoded = pcall(encodeJson, settings)
        file:write(result and encoded or "")
        file:close()
		print('[Vanguard Helper] Настройки сохранены!')
        return result
    else
        print('[Vanguard Helper] Не удалось сохранить настройки хелпера, ошибка: ', errstr)
        return false
    end
end
load_settings()
-------------------------------------------- JSON MY NOTES ---------------------------------------------
local default_notes = {
	note = {
		{ note_name = 'Тен-коды', note_text = '10-1 - Сбор всех офицеров на дежурстве.&10-2 - Вышел в патруль.&10-2R - Закончил патруль.&10-3 - Радиомолчание.&10-4 - Принято.&10-5 - Повторите.&10-6 - Не принято/неверно/нет.&10-7 - Ожидайте.&10-8 - Не доступен/занят.&10-14 - Запрос транспортировки.&10-15 - Подозреваемые арестованы.&10-18 - Требуется поддержка дополнительных юнитов.&10-20 - Локация.&10-21 - Статус и местонахождение.&10-22 - Выдвигайтесь к локации.&10-27 - Меняю маркировку патруля.&10-30 - Дорожно-транспортное происшествие.&10-40 - Большое скопление людей (более 4).&10-41 - Нелегальная активность.&10-46 - Провожу обыск.&10-55 - Траффик стоп.&10-57 VICTOR - Погоня за автомобилем.&10-57 FOXTROT - Пешая погоня.&10-66 - Траффик стоп повышенного риска.&10-70 - Запрос поддержки.&10-71 - Запрос медицинской поддержки.&10-88 - Теракт/ЧС.&10-99 - Ситуация урегулирована.&10-100 Временно недоступен для вызовов.' },
		{ note_name = 'Ситуационные коды', note_text = 'CODE 0 - Офицер ранен.&CODE 1 - Офицер в бедственном положении, нужна помощь всех юнитов.&CODE 2 - Обычный вызов [без сирен/стробоскопов/соблюдение ПДД].&CODE 2 HIGHT - Приоритетный вызов [без сирен/стробоскопов/соблюдение ПДД].&CODE 3 - Срочный вызов [сирены, стробоскопы, игнорирования ПДД].&CODE 4 - Стабильно, помощь не требуется.&CODE 4 ADAM - Помощь не требуется, но офицеры поблизости должны быть готовы оказать помощь.&CODE 5 - Офицерам держаться подальше от опасного места.&CODE 6 - Задерживаюсь на месте [включая локацию и причину,например, 911].&CODE 7 - Перерыв на обед.&CODE 30 - Срабатывание "тихой" сигнализации на месте происшествия.&CODE 30 RINGER - Срабатывание "громкой сигнализации на месте происшествия.&CODE 37 - Обнаружение угнанного транспортного средства.&CODE TOM - Офицеру требуется Тайзер.' },
		{ note_name = 'Маркировки патруля', note_text = 'Основные:&ADAM [A] - Патруль из 2/3 офицеров на крузере.&LINCOLN [L] - Одиночный патруль на крузере.&MARY [M] - Одиночный патруль на мотоцикле.&HENRY [H] - Высокоскоростой патруль.&AIR [AIR] - Воздушный патруль.&Air Support Division [ASD] - Воздушная поддержка.&&Дополнительные:&CHARLIE [C] - Группа захвата.&ROBERT [R] - Отдел Детективов.&SUPERVISOR [SV] - Руководящий состав.&DAVID [D] - Cпециальный отдел SWAT.&EDWARD [E] - Эвакуатор полиции.&NORA [N] - немаркированная единица патруля.',},
	}
}

local notes = {} -- пусто по умолчанию
local path_notes = configDirectory .. "/Notes.json"

function load_notes()
	if doesFileExist(path_notes) then
		local file, errstr = io.open(path_notes, 'r')
        if file then
            local contents = file:read('*a')
            file:close()
			if #contents == 0 then
				print('[Vanguard Helper] Файл заметок пуст. Загружаю дефолтные заметки.')
				notes = default_notes
				save_notes()
			else
				local result, loaded = pcall(decodeJson, contents)
				if result then
					notes = loaded
					print('[Vanguard Helper] Заметки инициализированы!')
				else
					print('[Vanguard Helper] Ошибка при чтении заметок. Загружаю дефолтные.')
					notes = default_notes
					save_notes()
				end
			end
        else
			print('[Vanguard Helper] Не удалось открыть файл с заметками. Загружаю дефолтные.')
			notes = default_notes
			save_notes()
        end
	else
		print('[Vanguard Helper] Файл заметок не найден. Создаю дефолтные.')
		notes = default_notes
		save_notes()
	end
end
function save_notes()
    local file, errstr = io.open(path_notes, 'w')
    if file then
        local result, encoded = pcall(encodeJson, notes)
        file:write(result and encoded or "")
        file:close()
		print('[Vanguard Helper] Заметки сохранены!')
        return result
    else
        print('[Vanguard Helper] Не удалось сохранить заметки, ошибка: ', errstr)
        return false
    end
end
load_notes()
-------------------------------------------- JSON RP GUNS ---------------------------------------------
local rp_guns = {
    {id = 0, name = 'кулаки', enable = true, rpTake = 2},
    {id = 1, name = 'кастеты', enable = true, rpTake = 2},
    {id = 2, name = 'клюшку для гольфа', enable = true, rpTake = 1},
    {id = 3, name = 'дубинку', enable = true, rpTake = 3},
    {id = 4, name = 'острый нож', enable = true, rpTake = 3},
    {id = 5, name = 'биту', enable = true, rpTake = 1},
    {id = 6, name = 'лопату', enable = true, rpTake = 1},
    {id = 7, name = 'кий', enable = true, rpTake = 1},
    {id = 8, name = 'катану', enable = true, rpTake = 1},
    {id = 9, name = 'бензопилу', enable = true, rpTake = 1},
    {id = 10, name = 'дидло', enable = true, rpTake = 2},
    {id = 11, name = 'дидло', enable = true, rpTake = 2},
    {id = 12, name = 'вибратор', enable = true, rpTake = 2},
    {id = 13, name = 'вибратор', enable = true, rpTake = 2},
    {id = 14, name = 'букет цветов', enable = true, rpTake = 1},
    {id = 15, name = 'трость', enable = true, rpTake = 1},
    {id = 16, name = 'осколочную гранату', enable = true, rpTake = 3},
    {id = 17, name = 'дымовую гранату', enable = true, rpTake = 3},
    {id = 18, name = 'коктейль Молотова', enable = true, rpTake = 3},
    {id = 22, name = 'пистолет Colt45', enable = true, rpTake = 4},
    {id = 23, name = "электрошокер Taser-X26P", enable = true, rpTake = 4},
    {id = 24, name = 'пистолет Desert Eagle', enable = true, rpTake = 4},
    {id = 25, name = 'дробовик', enable = true, rpTake = 1},
    {id = 26, name = 'обрез', enable = true, rpTake = 1},
    {id = 27, name = 'улучшенный обрез', enable = true, rpTake = 1},
    {id = 28, name = 'пистолет-пулемёт Micro Uzi', enable = true, rpTake = 4},
    {id = 29, name = 'пистолет-пулемёт MP5', enable = true, rpTake = 4},
    {id = 30, name = 'автомат AK-47', enable = true, rpTake = 1},
    {id = 31, name = 'автомат M4', enable = true, rpTake = 1},
    {id = 32, name = 'пистолет-пулемёт Tec-9', enable = true, rpTake = 4},
    {id = 33, name = 'винтовку Rifle', enable = true, rpTake = 1},
    {id = 34, name = 'снайперскую винтовку Rifle', enable = true, rpTake = 1},
    {id = 35, name = 'ручную противотанковую ракету', enable = true, rpTake = 1},
    {id = 36, name = 'устройство для запуска ракет', enable = true, rpTake = 1},
    {id = 37, name = 'огнемёт', enable = true, rpTake = 1},
    {id = 38, name = 'миниган', enable = true, rpTake = 1},
    {id = 39, name = 'динамит', enable = true, rpTake = 3},
    {id = 40, name = 'детонатор', enable = true, rpTake = 3},
    {id = 41, name = 'перцовый балончик', enable = true, rpTake = 2},
    {id = 42, name = 'огнетушитель', enable = true, rpTake = 1},
    {id = 43, name = 'фотоапарат', enable = true, rpTake = 2},
    {id = 44, name = 'прибор ночного видения', enable = true, rpTake = 2},
    {id = 45, name = 'тепловизор', enable = true, rpTake = 2},
    {id = 46, name = 'ручной парашут', enable = true, rpTake = 1},
    -- gta sa damage reason
    {id = 49, name = 'автомобиль', rpTake = 1},
    {id = 50, name = 'лопасти вертолёта', rpTake = 1},
    {id = 51, name = 'бомбу', rpTake = 1},
    {id = 54, name = 'коллизию', rpTake = 1},
	-- ARZ CUSTOM GUN
    {id = 71, name = 'пистолет Desert Eagle Steel', enable = true, rpTake = 4},
    {id = 72, name = 'пистолет Desert Eagle Gold', enable = true, rpTake = 4},
    {id = 73, name = 'пистолет Glock Gradient', enable = true, rpTake = 4},
    {id = 74, name = 'пистолет Desert Eagle Flame', enable = true, rpTake = 4},
    {id = 75, name = 'пистолет Python Royal', enable = true, rpTake = 4},
    {id = 76, name = 'пистолет Python Silver', enable = true, rpTake = 4},
    {id = 77, name = 'автомат AK-47 Roses', enable = true, rpTake = 1},
    {id = 78, name = 'автомат AK-47 Gold', enable = true, rpTake = 1},
    {id = 79, name = 'пулемёт M249 Graffiti', enable = true, rpTake = 1},
    {id = 80, name = 'золотую Сайгу', enable = true, rpTake = 1},
    {id = 81, name = 'пистолет-пулемёт Standart', enable = true, rpTake = 4},
    {id = 82, name = 'пулемёт M249', enable = true, rpTake = 1},
    {id = 83, name = 'пистолет-пулемёт Skorp', enable = true, rpTake = 4},
    {id = 84, name = 'автомат AKS-74 камуфляжный', enable = true, rpTake = 1},
    {id = 85, name = 'автомат AK-47 камуфляжный', enable = true, rpTake = 1},
    {id = 86, name = 'дробовик Rebecca', enable = true, rpTake = 1},
    {id = 87, name = 'портальную пушку', enable = true, rpTake = 1},
    {id = 88, name = 'ледяной меч', enable = true, rpTake = 1},
    {id = 89, name = 'портальную пушку', enable = true, rpTake = 4},
    {id = 90, name = 'оглушающую гранату', enable = true, rpTake = 3},
    {id = 91, name = 'ослепляющую гранату', enable = true, rpTake = 3},
	{id = 92, name = 'снайперскую винтовку McMillian TAC-50', enable = true, rpTake = 1},
	{id = 93, name = 'оглушающий пистолет', enable = true, rpTake = 4},
}
local rpTakeNames = {{"из-за спины", "за спину"}, {"из кармана", "в карман"}, {"из пояса", "на пояс"}, {"из кобуры", "в кобуру"}}  
local path_rp_guns = configDirectory .. "/rp_guns.json"
function load_rp_guns()
	if doesFileExist(path_rp_guns) then
		local file, errstr = io.open(path_rp_guns, 'r')
        if file then
            local contents = file:read('*a')
            file:close()
			if #contents == 0 then
				print('[Vanguard Helper] Не удалось открыть файл с рп ганами!')
				print('[Vanguard Helper] Причина: этот файл пустой')
			else
				local result, loaded = pcall(decodeJson, contents)
				if result then
					rp_guns = loaded
					print('[Vanguard Helper] Рп ганы инициализированы!')
				else
					print('[Vanguard Helper] Не удалось открыть файл с с рп ганами!')
					print('[Vanguard Helper] Причина: Не удалось декодировать json (ошибка в файле)')
				end
			end
        else
			print('[Vanguard Helper] Не удалось открыть файл с rp ганами!')
			print('[Vanguard Helper] Причина: ')
        end
	else
		print('[Vanguard Helper] Не удалось открыть файл с с рп ганами!')
		print('[Vanguard Helper] Причина: этого файла нету в папке '..configDirectory)
	end
end
function save_rp_guns()
    local file, errstr = io.open(path_rp_guns, 'w')
    if file then
        local result, encoded = pcall(encodeJson, rp_guns)
        file:write(result and encoded or "")
        file:close()
		print('[Vanguard Helper] Рп ганы сохранены!')
        return result
    else
        print('[Vanguard Helper] Не удалось сохранить рп ганы, ошибка: ', errstr)
        return false
    end
end
load_rp_guns()
-------------------------------------------- JSON SMART UK ---------------------------------------------
local smart_uk = {}
local path_uk = configDirectory .. "/SmartUK.json"
function load_smart_uk()
	if doesFileExist(path_uk) then
		local file, errstr = io.open(path_uk, 'r')
        if file then
            local contents = file:read('*a')
            file:close()
			if #contents == 0 then
				print('[Vanguard Helper] Не удалось открыть файл с умным розыском!')
				print('[Vanguard Helper] Причина: этот файл пустой')
			else
				local result, loaded = pcall(decodeJson, contents)
				if result then
					smart_uk = loaded
					print('[Vanguard Helper] Умный розыск инициализирован!')
				else
					print('[Vanguard Helper] Не удалось открыть файл с умным розыском!')
					print('[Vanguard Helper] Причина: Не удалось декодировать json (ошибка в файле)')
				end
			end
        else
			print('[Vanguard Helper] Не удалось открыть файл с умным розыском!')
			print('[Vanguard Helper] Причина: ')
        end
	else
		print('[Vanguard Helper] Не удалось открыть файл с умным розыском!')
		print('[Vanguard Helper] Причина: этого файла нету в папке '..configDirectory)
	end
end
function save_smart_uk()
    local file, errstr = io.open(path_uk, 'w')
    if file then
        local result, encoded = pcall(encodeJson, smart_uk)
        file:write(result and encoded or "")
        file:close()
		print('[Vanguard Helper] Умный розыск сохранён!')
        return result
    else
        print('[Vanguard Helper] Не удалось сохранить умный розыск, ошибка: ', errstr)
        return false
    end
end
load_smart_uk()
-------------------------------------------- JSON SMART PDD ---------------------------------------------
local smart_pdd = {}
local path_pdd = configDirectory .. "/SmartPDD.json"
function load_smart_pdd()
	if doesFileExist(path_pdd) then
		local file, errstr = io.open(path_pdd, 'r')
        if file then
            local contents = file:read('*a')
            file:close()
			if #contents == 0 then
				print('[Vanguard Helper] Не удалось открыть файл с умным штрафом!')
				print('[Vanguard Helper] Причина: этот файл пустой')
			else
				local result, loaded = pcall(decodeJson, contents)
				if result then
					smart_pdd = loaded
					print('[Vanguard Helper] Умный штраф инициализирован!')
				else
					print('[Vanguard Helper] Не удалось открыть файл с умным штрафом!')
					print('[Vanguard Helper] Причина: Не удалось декодировать json (ошибка в файле)')
				end
			end
        else
			print('[Vanguard Helper] Не удалось открыть файл с умным штрафом!')
			print('[Vanguard Helper] Причина: ', errstr)
        end
	else
		print('[Vanguard Helper] Не удалось открыть файл с умным штрафом!')
		print('[Vanguard Helper] Причина: этого файла нету в папке '..configDirectory)
	end
end
function save_smart_pdd()
    local file, errstr = io.open(path_pdd, 'w')
    if file then
        local result, encoded = pcall(encodeJson, smart_pdd)
        file:write(result and encoded or "")
        file:close()
		print('[Vanguard Helper] Умные штрафы сохранены!')
        return result
    else
        print('[Vanguard Helper] Не удалось сохранить умные штрафы, ошибка: ', errstr)
        return false
    end
end
load_smart_pdd()
-------------------------------------------- JSON COMMANDS ---------------------------------------------
local commands = {
	commands = {
		{ cmd = '55', description = 'Проведение 10-55', text = '/m Водитель{get_storecar_model}, внимание!&/m Немедленно снизьте скорость и прижмитесь к обочине!&/m После остановки заглушите двигатель, держите руки на руле и не выходите из транспорта.&/m В случае неподчинения вы будете 
