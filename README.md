CREATE SCHEMA `delivery` ; %создание базы данных "delivery"

CREATE TABLE `delivery`.`production` (

  `id` INT NOT NULL AUTO_INCREMENT,

  `name` VARCHAR(45) NULL DEFAULT NULL,

  `price` DECIMAL(7,2) NULL DEFAULT NULL,

  PRIMARY KEY (`id`)); %создание таблицы продуктов

INSERT INTO `delivery`.`production` (`name`, `price`) VALUES ('Сыр осетинский(кг)', '250');

INSERT INTO `delivery`.`production` (`name`, `price`) VALUES ('Говяжья вырезка(кг)', '400');

INSERT INTO `delivery`.`production` (`name`, `price`) VALUES ('Молоко(л)', '60');

INSERT INTO `delivery`.`production` (`name`, `price`) VALUES ('Яйца(10 шт) кат С0', '50');

INSERT INTO `delivery`.`production` (`name`, `price`) VALUES ('Яйца(10 шт) кат С1', '30'); %её заполнение продуктами

CREATE TABLE `delivery`.`couriers` (

  `id` INT NOT NULL AUTO_INCREMENT,

  `name` VARCHAR(45) NULL,

  `Number` VARCHAR(45) NULL,

  `carModel` VARCHAR(45) NULL,

  PRIMARY KEY (`id`)); %ввод таблицы курьеров

INSERT INTO `delivery`.`couriers` (`id`, `name`, `Number`, `carModel`) VALUES ('3', 'Олег', '89888152255', 'лада самара');

INSERT INTO `delivery`.`couriers` (`id`, `name`, `Number`, `carModel`) VALUES ('1', 'Дмитрий', '89065551233', 'лада ларгус');

INSERT INTO `delivery`.`couriers` (`id`, `name`, `Number`, `carModel`) VALUES ('2', 'Алексей', '89604052211', 'ваз 2102'); %заполнили её

CREATE TABLE `delivery`.`orders` (

  `id` INT NOT NULL AUTO_INCREMENT,

  `name` VARCHAR(45) NOT NULL,

  `address` VARCHAR(45) NOT NULL,

  `courierId` INT NOT NULL,

  PRIMARY KEY (`id`)); %создали таблицу заказов

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('ихаил', 'пр Доватора 30', '1');

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('Екатерина', 'пр Коста 42', '1');

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('Ахмед', 'ул Грибоедова 5', '2');

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('Хетаг', 'ул Владикавказская 31', '2');

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('Ольга', 'пр Мира 32', '3'); %заполнили таблицу заказов

ALTER TABLE `delivery`.`orders` 
ADD INDEX `orders_couriers_idx` (`courierId` ASC) VISIBLE;


ALTER TABLE `delivery`.`orders` 

ADD CONSTRAINT `orders_couriers`

  FOREIGN KEY (`courierId`)
  REFERENCES `delivery`.`couriers` (`id`)

  ON DELETE NO ACTION

  ON UPDATE NO ACTION; %привязали параметр courierId к id из couriers

CREATE TABLE `delivery`.`orderitems` (

  `id` INT NOT NULL AUTO_INCREMENT,

  `productId` INT NOT NULL,

  `orderId` INT NOT NULL,

  `amount` INT NOT NULL,

  PRIMARY KEY (`id`)); %создали таблицу с данными заказов

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('1', '1', '5');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('4', '1', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('3', '1', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('2', '2', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('3', '2', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('4', '2', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('1', '3', '6');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('2', '3', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('1', '4', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('5', '4', '3');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('1', '5', '2');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('2', '5', '1'); %заполнили эту таблицу id данных

ALTER TABLE `delivery`.`orderitems` 
ADD INDEX `orders_orderItems_idx` (`orderId` ASC) VISIBLE,

ADD INDEX `production_orderItems_idx` (`productId` ASC) VISIBLE;
;
ALTER TABLE `delivery`.`orderitems`
 
ADD CONSTRAINT `orders_orderItems`

  FOREIGN KEY (`orderId`)

  REFERENCES `delivery`.`orders` (`id`)

  ON DELETE NO ACTION

  ON UPDATE NO ACTION,

ADD CONSTRAINT `production_orderItems`

  FOREIGN KEY (`productId`)

  REFERENCES `delivery`.`production` (`id`)

  ON DELETE NO ACTION

  ON UPDATE NO ACTION; %создали связь orderitems>orders orderItems>production

Это была настройка таблиц, переходим к задачам


Пункт 1 >>>>>

select o.name,o.address,c.name,p.name,oi.amount,p.price*oi.amount as Sum 

from delivery.orders as o

join delivery.couriers as c

on o.courierId=c.id

join delivery.orderitems as oi

on o.id=oi.orderId

join delivery.production as p

on p.id=oi.productId

where o.id=2 <<<< выведет данные заказа с id=2


Пункт 2 >>>>>

INSERT INTO `delivery`.`orders` (`name`, `address`, `courierId`) VALUES ('Татьяна', 'ул Цоколаева 18', '3');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('1', '6', '1');

INSERT INTO `delivery`.`orderitems` (`productId`, `orderId`, `amount`) VALUES ('3', '6', '2'); <<создание нового заказа


Пункт 3 SELECT * FROM delivery.orders where orders.courierId=2; %выберет все заказы с id=2

Пункт 4 UPDATE `delivery`.`orderitems` SET `amount` = '6' WHERE (`id` = '1'); %изменение ячейки с количеством
