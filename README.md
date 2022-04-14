# Exchange_WSR
Обмен между базами 1С средствами веб сервиса

# Контекст
+ 1С:Предприятие 8.3 (8.3.20.1674)
+ Расширение
+ Основной режим запуска "Управляемое приложение"
+ Режим совместимости Версия 8.3.17

# Назначение
Расширение подключается в конфигурацию приемник (запроса)
и обрабатывает входящий запрос средствами web service

# Использование
// 1. Публикация вебсервиса "KernelReceiver"
// 2. В какой либо базе реализовать метод "ПослатьЗапрос"

// Функция - Послать запрос
//
// Параметры:
//  ТекстЗапроса	 - Строка - Описывает запрос к данным.
//  Параметры		 - Структура - Параметры для запроса.
//  ТипОбходаЗапроса - Строка - "ПоГруппировкам", ПоГруппировкамСИерархией", "Прямой"
//	(это тип "ОбходРезультатаЗапроса" так как на стороне ws сервера будет выполнена Выгрузка запроса)
// 
// Возвращаемое значение:
//  Ответ - ЗначениеИзСтрокиВнутр - Произвольный
//
Функция ПослатьЗапросЗУП(ТекстЗапроса, Параметры = "", ТипОбходаЗапроса = "Прямой") Экспорт

	ИмяСервера 			= "http://192.168.88.203:8012/"; 
	ИмяБазы 			= "zup";
	ИмяWS 				= "KernelReceiver";
    ПространствоИмен 	= "http://www.KernelReceiver_zup.com";

	WSОпределение 		= Новый WSОпределения(ИмяСервера + ИмяБазы + "/ws/" + ИмяWS + ".1cws/?wsdl");
	Прокси 				= Новый WSПрокси(WSОпределение, ПространствоИмен , ИмяWS, ИмяWS + "Soap");
	    
    стрПараметры 		= ЗначениеВСтрокуВнутр(Параметры);
    ОтветWS 			= Прокси.ВыполнитьЗапрос(ТекстЗапроса, стрПараметры, ТипОбходаЗапроса);

    Ответ = ЗначениеИзСтрокиВнутр(ОтветWS);   
	
	Возврат Ответ;
	
КонецФункции
