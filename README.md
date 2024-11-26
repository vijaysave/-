-- Таблица типов климатической техники
CREATE TABLE ClimateTechTypes (
    TypeID INT PRIMARY KEY IDENTITY(1,1),
    TypeName NVARCHAR(50) NOT NULL
);

-- Таблица моделей климатической техники
CREATE TABLE ClimateTechModels (
    ModelID INT PRIMARY KEY IDENTITY(1,1),
    ModelName NVARCHAR(100) NOT NULL,
    TypeID INT NOT NULL,
    FOREIGN KEY (TypeID) REFERENCES ClimateTechTypes(TypeID)
);

-- Таблица статусов заявок
CREATE TABLE Statuses (
    StatusID INT PRIMARY KEY IDENTITY(1,1),
    StatusName NVARCHAR(50) NOT NULL
);

-- Таблица пользователей
CREATE TABLE Users (
    UserID INT PRIMARY KEY IDENTITY(1,1),
    FIO NVARCHAR(100) NOT NULL,
    Phone NVARCHAR(20),
    Login NVARCHAR(50),
    Password NVARCHAR(50),
    UserType NVARCHAR(50)
);

-- Таблица заявок
CREATE TABLE Requests (
    RequestID INT PRIMARY KEY IDENTITY(1,1),
    StartDate DATE NOT NULL,
    ModelID INT NOT NULL,
    ProblemDescription NVARCHAR(MAX),
    StatusID INT NOT NULL,
    CompletionDate DATE NULL,
    MasterID INT NULL,
    ClientID INT NOT NULL,
    FOREIGN KEY (ModelID) REFERENCES ClimateTechModels(ModelID),
    FOREIGN KEY (StatusID) REFERENCES Statuses(StatusID),
    FOREIGN KEY (MasterID) REFERENCES Users(UserID),
    FOREIGN KEY (ClientID) REFERENCES Users(UserID)
);

-- Таблица деталей
CREATE TABLE Parts (
    PartID INT PRIMARY KEY IDENTITY(1,1),
    PartName NVARCHAR(100) NOT NULL
);

-- Таблица комментариев к заявкам
CREATE TABLE Comments (
    CommentID INT PRIMARY KEY IDENTITY(1,1),
    Message NVARCHAR(MAX),
    MasterID INT NOT NULL,
    RequestID INT NOT NULL,
    FOREIGN KEY (MasterID) REFERENCES Users(UserID),
    FOREIGN KEY (RequestID) REFERENCES Requests(RequestID)
);
go 
-- Заполнение типов климатической техники
INSERT INTO ClimateTechTypes (TypeName) VALUES 
('Кондиционер'),
('Увлажнитель воздуха'),
('Сушилка для рук');

-- Заполнение моделей климатической техники
INSERT INTO ClimateTechModels (ModelName, TypeID) VALUES 
('TCL TAC-12CHSA/TPG-W белый', 1),
('Electrolux EACS/I-09HAT/N3_21Y белый', 1),
('Xiaomi Smart Humidifier 2', 2),
('Polaris PUH 2300 WIFI IQ Home', 2),
('Ballu BAHD-1250', 3);

-- Заполнение статусов
INSERT INTO Statuses (StatusName) VALUES 
('Новая заявка'),
('В процессе ремонта'),
('Готова к выдаче');

-- Заполнение пользователей
INSERT INTO Users (FIO, Phone, Login, Password, UserType) VALUES
('Широков Василий Матвеевич', '89210563128', 'login1', 'pass1', 'Менеджер'),
('Кудрявцева Ева Ивановна', '89535078985', 'login2', 'pass2', 'Специалист'),
('Гончарова Ульяна Ярославовна', '89210673849', 'login3', 'pass3', 'Специалист'),
('Гусева Виктория Данииловна', '89990563748', 'login4', 'pass4', 'Оператор'),
('Баранов Артём Юрьевич', '89994563847', 'login5', 'pass5', 'Оператор'),
('Овчинников Фёдор Никитич', '89219567849', 'login6', 'pass6', 'Заказчик'),
('Петров Никита Артёмович', '89219567841', 'login7', 'pass7', 'Заказчик'),
('Ковалева Софья Владимировна', '89219567842', 'login8', 'pass8', 'Заказчик'),
('Кузнецов Сергей Матвеевич', '89219567843', 'login9', 'pass9', 'Заказчик'),
('Беспалова Екатерина Даниэльевна', '89219567844', 'login10', 'pass10', 'Специалист');

-- Заполнение заявок
INSERT INTO Requests (StartDate, ModelID, ProblemDescription, StatusID, CompletionDate, MasterID, ClientID) VALUES
('2023-06-06', 1, 'Не охлаждает воздух', 2, NULL, 2, 7),
('2023-05-05', 2, 'Выключается сам по себе', 2, NULL, 3, 8),
('2022-07-07', 3, 'Пар имеет неприятный запах', 3, '2023-01-01', 3, 9),
('2023-08-02', 4, 'Увлажнитель воздуха продолжает работать при предельном снижении уровня воды', 1, NULL, NULL, 8),
('2023-08-02', 5, 'Не работает', 1, NULL, NULL, 9);

-- Заполнение комментариев
INSERT INTO Comments (Message, MasterID, RequestID) VALUES
('Всё сделаем!', 2, 1),
('Всё сделаем!', 3, 2),
('Починим в момент.', 3, 3);
