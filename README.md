# test-telegram
<?php
/**
 * infokr_bot
 * https://toster.ru/q/516412
 */
header('Content-Type: text/html; charset=utf-8');
// подрубаем API
require_once("vendor/autoload.php");

// дебаг
if(true){
	error_reporting(E_ALL & ~(E_NOTICE | E_USER_NOTICE | E_DEPRECATED));
	ini_set('display_errors', 1);
}

// создаем переменную бота
$token = "562806471:AAGWUQh5GCMTSM-k4tYXGREg6ShIyVCHRuU";
$bot = new \TelegramBot\Api\Client($token,null);

if($_GET["bname"] == "revcombot"){
	$bot->sendMessage("@burgercaputt", "Тест");
}

// если бот еще не зарегистрирован - регистируем
if(!file_exists("registered.trigger")){ 
	/**
	 * файл registered.trigger будет создаваться после регистрации бота. 
	 * если этого файла нет значит бот не зарегистрирован 
	 */
	 
	// URl текущей страницы
	$page_url = "https://".$_SERVER["SERVER_NAME"].$_SERVER["REQUEST_URI"];
	$result = $bot->setWebhook($page_url);
	if($result){
		file_put_contents("registered.trigger",time()); // создаем файл дабы прекратить повторные регистрации
	} else die("ошибка регистрации");
}
/**
// Команды бота
// пинг. Тестовая
$bot->command('ping', function ($message) use ($bot) {
	$bot->sendMessage($message->getChat()->getId(), 'pong!');
});
*/
// Запуск бота
$bot->command('start', function ($message) use ($bot) {
    $answer = 'Вас приветствует
🇦🇷Td🇦🇷

Покупая у нас, Вы получаете:
✅ Честный СЕРВИС. 
У нас электроприборы - это электроприборы. Холодильники, стиралки, микроволновки!  Реально! благодаря уникальному способу упаковки товара!.
✅ Выгодный СЕРВИС.
Цена всегда соответствует качеству товара и уровню работы сервиса.
✅ Качественный СЕРВИС.
В прайс добавляются товары производства ТОП фирм Лджи, Рейнфорд, Самсунг!.

Почему стоит выбрать Наш магазин:
✅ Вероятность успешно найти свой Td максимально приближена к 100%.
✅ Служба поддержки, которая способна ответить на любые возникшие вопросы доступна 24/7. 
✅ В Нашем магазине покупать выгоднее, спокойнее и дешевле.
	
Вы в безопасности:
✅ Все сделки проводятся только по правилам анонимности. С использованием шифрования данных на сервере бота.
✅ Данные о текущих сделках зашифрованы и удаляются в течении дня после завершения.
⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆
Если бот не отвечает наберите комманду
/start
⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆
✔️Ежедневная техподдержка и опт:
📞 telegram: @Td
✔️В случае удаления бота резервныe адресa:
📞 telegram: @Td_bot
✔️Телеграм канал: 
📞 @999
⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆
⇝Акция по ****** :
⇝ 10 из 100 холодильников c 1g, имеют уулучшенные характеристики!
☘️Испытай свою удачу! ☘️
⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆⋆
 Скоро в Наличии!!!: - Ice (Рейнфорд)
А так же Впервые в городе:
- Холодильник
- Электроплита
Выберите Кривой Рог⬇️
';
	$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Кривой Рог"]]], true, true);
    $bot->sendMessage($message->getChat()->getId(), $answer, false, null,null, $keyboard);
});

// помощ
$bot->command('help', function ($message) use ($bot) {
    $answer = 'Команды:
/help - помощ';
    $bot->sendMessage($message->getChat()->getId(), $answer);
});

// передаем картинку
$bot->command('getpic', function ($message) use ($bot) {
	$pic = "http://aftamat4ik.ru/wp-content/uploads/2017/03/photo_2016-12-13_23-21-07.jpg";

    $bot->sendPhoto($message->getChat()->getId(), $pic);
});

// передаем документ
$bot->command('getdoc', function ($message) use ($bot) {
	$document = new \CURLFile('shtirner.txt');

    $bot->sendDocument($message->getChat()->getId(), $document);
});

// Кнопки у сообщений
$bot->command("ibutton", function ($message) use ($bot) {
	$keyboard = new \TelegramBot\Api\Types\Inline\InlineKeyboardMarkup(
		[
			[
				['callback_data' => 'data_test', 'text' => 'Answer'],
				['callback_data' => 'data_test2', 'text' => 'ОтветЪ']
			]
		]
	);

	$bot->sendMessage($message->getChat()->getId(), "тест", false, null,null,$keyboard);
});

// Обработка кнопок у сообщений
$bot->on(function($update) use ($bot, $callback_loc, $find_command){
	$callback = $update->getCallbackQuery();
	$message = $callback->getMessage();
	$chatId = $message->getChat()->getId();
	$data = $callback->getData();
	
	if($data == "data_test"){
		$bot->answerCallbackQuery( $callback->getId(), "This is Ansver!",true);
	}
	if($data == "data_test2"){
		$bot->sendMessage($chatId, "Это ответ!");
		$bot->answerCallbackQuery($callback->getId()); // можно отослать пустое, чтобы просто убрать "часики" на кнопке
	}

}, function($update){
	$callback = $update->getCallbackQuery();
	if (is_null($callback) || !strlen($callback->getData()))
		return false;
	return true;
});

// обработка инлайнов
$bot->inlineQuery(function ($inlineQuery) use ($bot) {
	mb_internal_encoding("UTF-8");
	$qid = $inlineQuery->getId();
	$text = $inlineQuery->getQuery();
	
	// Это - базовое содержимое сообщения, оно выводится, когда тыкаем на выбранный нами инлайн
	$str = "Что другие?
Свора голодных нищих.
Им все равно...
В этом мире немытом
Душу человеческую
Ухорашивают рублем,
И если преступно здесь быть бандитом,
То не более преступно,
Чем быть королем...
Я слышал, как этот прохвост
Говорил тебе о Гамлете.
Что он в нем смыслит?
<b>Гамлет</b> восстал против лжи,
В которой варился королевский двор.
Но если б теперь он жил,
То был бы бандит и вор.";
	$base = new \TelegramBot\Api\Types\Inline\InputMessageContent\Text($str,"Html");
	
	// Это список инлайнов
	// инлайн для стихотворения
	$msg = new \TelegramBot\Api\Types\Inline\QueryResult\Article("1","С. Есенин","Отрывок из поэмы `Страна негодяев`");
	$msg->setInputMessageContent($base); // указываем, что в ответ к этому сообщению надо показать стихотворение
	
	// инлайн для картинки
	$full = "http://aftamat4ik.ru/wp-content/uploads/2017/05/14277366494961.jpg"; // собственно урл на картинку 
	$thumb = "http://aftamat4ik.ru/wp-content/uploads/2017/05/14277366494961-150x150.jpg"; // и миниятюра
	
	$photo = new \TelegramBot\Api\Types\Inline\QueryResult\Photo("2",$full,$thumb);
	
	// инлайн для музыки
	$url = "http://aftamat4ik.ru/wp-content/uploads/2017/05/mongol-shuudan_-_kozyr-nash-mandat.mp3";
	$mp3 = new \TelegramBot\Api\Types\Inline\QueryResult\Audio("3",$url,"Монгол Шуудан - Козырь наш Мандат!");
	
	// инлайн для видео
	$vurl = "http://aftamat4ik.ru/wp-content/uploads/2017/05/bb.mp4";
	$thumb = "http://aftamat4ik.ru/wp-content/uploads/2017/05/joker_5-150x150.jpg";
	$video = new \TelegramBot\Api\Types\Inline\QueryResult\Video("4",$vurl,$thumb, "video/mp4","коммунальные службы","тут тоже может быть описание");
	
	// отправка
	try{
		$result = $bot->answerInlineQuery( $qid, [$msg,$photo,$mp3,$video],100,false);
	}catch(Exception $e){
		file_put_contents("rdata",print_r($e,true));
	}
});

// Reply-Кнопки
$bot->command("buttons", function ($message) use ($bot) {
	$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Власть советам!\n"], ["text" => "Сиськи!"]]], true, true);

	$bot->sendMessage($message->getChat()->getId(), "тест", false, null,null, $keyboard);
});

// Отлов любых сообщений + обрабтка reply-кнопок
$bot->on(function($Update) use ($bot){
	
	$message = $Update->getMessage();
	$mtext = $message->getText();
	$cid = $message->getChat()->getId();
	
	if(mb_stripos($mtext,"Кривой Рог") !== false){
		$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Купить Td"], ["text" => "Работа"]]], true, true);
		$bot->sendMessage($message->getChat()->getId(), "1.Купить Td
2.Нужна Работа 
", false, null,null, $keyboard);
	}
	if(mb_stripos($mtext,"Работа") !== false){
		$pic = "http://aftamat4ik.ru/wp-content/uploads/2017/05/14277366494961.jpg";

		$bot->sendPhoto($message->getChat()->getId(), $pic);
	}
	if(mb_stripos($mtext,"Купить Td") !== false){
		$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Nature Blueberry (19,5% ТГК)"], ["text" => "Nature White Widow (19 % ТГК)"], ["text" => "Nature AK 47 (23% ТГК)"], ["text" => "Nature Amnesia (19 % ТГК)"]]], true, true);
		$bot->sendMessage($message->getChat()->getId(), "➖➖➖➖➖➖➖➖➖➖
✔️ В случае удаления бота резервныe адресa:
📞 telegram: @999
➖➖➖➖➖➖➖➖➖➖
🏠 Кривой Рог
🎁 Выберите продукт
", false, null,null, $keyboard);
	}
	if(mb_stripos($mtext,"Nature Blueberry (19,5% ТГК)") !== false){
		$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Заказать"], ["text" => "Назад"]]], true, true);
		$bot->sendMessage($message->getChat()->getId(), "➖➖➖➖➖➖➖➖➖➖
✔️ В случае удаления бота резервныe адресa:
✔️ BCUBIZ $$$
✔️ WTF $$$
➖➖➖➖➖➖➖➖➖➖
🏠 Кривой Рог
🎁 Выбранный продукт Nature Blueberry (19,5% ТГК)
", false, null,null, $keyboard);
	}
	if(mb_stripos($mtext,"Назад") !== false){
		$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Nature Blueberry (19,5% ТГК)"], ["text" => "Nature White Widow (19 % ТГК)"], ["text" => "Nature AK 47 (23% ТГК)"], ["text" => "Nature Amnesia (19 % ТГК)"]]], true, true);
		$bot->sendMessage($message->getChat()->getId(), "➖➖➖➖➖➖➖➖➖➖
✔️ В случае удаления бота резервныe адресa:
📞 telegram: @999
➖➖➖➖➖➖➖➖➖➖
🏠 Кривой Рог
🎁 Выберите продукт
", false, null,null, $keyboard);
	}
		if(mb_stripos($mtext,"Заказать") !== false){
		$keyboard = new \TelegramBot\Api\Types\ReplyKeyboardMarkup([[["text" => "Отменить заказ!"]]], true, true);
		$bot->sendMessage($message->getChat()->getId(), "➖➖➖➖➖➖➖➖➖➖
✔️ Проведите оплату:
📞 telegram: @999
➖➖➖➖➖➖➖➖➖➖
🏠 Алгоритм оплаты и платежей
", false, null,null, $keyboard);
	}
	if(mb_stripos($mtext,"Отменить заказ!") !== false){
		$bot->sendMessage($message->getChat()->getId(), "Заказ отменен!
➖➖➖➖➖➖➖➖➖➖
✔️ Используйте команду /start, для начала переписки");
	}
}, function($message) use ($name){
	return true; // когда тут true - команда проходит
});

// функция отправки
if(isset($_GET["sendmsg"])){
	$data = file_get_contents("uids");
	$uids = ($data) ? json_decode($data,true) : array();
	foreach($uids as $cid){
		$bot->sendMessage($cid, "ХУЙ","html"); 
	}
}

$bot->on(function($Update) use ($bot){
	$message = $Update->getMessage();
	$mtext = $message->getText();
	$cid = $message->getChat()->getId();
	$data = file_get_contents("uids");
	$uids = ($data) ? json_decode($data,true) : array();
	if(!in_array($cid,$uids)){
		$uids[] = $cid;
		file_put_contents("uids",json_encode($uids));
	}
	
}, function($message) use ($name){
	return true; // когда тут true - команда проходит
});

// запускаем обработку
$bot->run();

echo "бот";
