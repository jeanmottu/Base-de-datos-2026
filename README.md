Ejercicio 1 Creación y poblado de tablas.

CREATE DATABASE Base_de_datos;
GO

USE Base_de_datos;
GO

CREATE TABLE Persona (
    CI INT IDENTITY(1,1) NOT NULL PRIMARY KEY,
    Nombre NVARCHAR(10) NULL,
    Apellido NVARCHAR(6) NULL,
    Telefono NUMERIC(10,0) NOT NULL
);


INSERT INTO Persona (Nombre, Apellido, Telefono)
VALUES ('Juan', 'Carlos', 23455667);

Select * from Persona;

Ejercicio 2 25-03-2026 Idem del ejercicio pasado.

TABLA DE VETERINARIA
create database Veterinaria
go

USE Veterinaria

create table dueño(
id_dueño int identity (1,1) primary key,
nombre NVARCHAR(80) NOT NULL,
email NVARCHAR (150)
);

go

create table Mascotas(
id_mascotas int identity (1,1) Primary Key,
nombre Nvarchar (80) not null,
especie nvarchar (50) NOT NULL,
nacimiento date,
id_dueño int NOT NULL,
CONSTRAINT fk_dueño FOREIGN KEY([id_dueño]) REFERENCES dueño(id_dueño)
);
go

Insert into dueño (nombre,email) VALUES ('Ana Garcia', 'ana@gmail.com'),
('Luis Perez', 'luis@gmail.com'), ('Diego' , 'diego@gmail.com')

Insert into Mascotas (nombre, especie, nacimiento,id_dueño) VALUES
('Rex' , 'Perro', '2020-03-15',1),
('Mia', 'Gato','2021-07-01',1),
('Mejia','therian', '2021-02-01',2);

select * from dueño;

select * from Mascotas;

Select * from Mascotas
where especie ='Perro'

Select M.nombre as mascotas , M.especie , D.nombre as dueño 
from Mascotas m
join dueño d on M.id_dueño = D.id_dueño;

select m.nombre as Mascota m.especie , D.nombre as dueño
from Mascotas m, dueño d
where m.id_dueño = d.id_dueño

select top 1 * from Mascotas

select nombre, ISNULL(email, 'sin email') as email 
from dueño

select nombre, 
datediff (DAY, nacimiento , getdate()) as dias
from mascotas

Ejercicio 3 8 de mayo Ejercicio de consultas y de uso de conectores lógicos.

select especie, count (*) as cantidad
from mascotas
group by especie

select * from dueño

select D.nombre, count (*) as cantidad_mascotas
from mascotas M
join dueño D on M.id_dueño = D.id_dueño
group by D.nombre
having count(*)>1;

select * from mascotas

CREATE TABLE Consultas( 
id_consulta INT Identity (1,1)Primary key,
id_mascotas INT NOT NULL,
fecha Datetime default getdate(),
motivo nvarchar(200),
costo DECIMAL (8,2) NOT NULL,
CONSTRAINT fk_mascota FOREIGN KEY (id_mascotas) REFERENCES Mascotas(id_mascotas)
);

delete from Consultas


INSERT INTO Consultas (id_mascotas, motivo, costo) VALUES 
(1,'Vacuna anual',850.00),
(2,'Control de peso',400.00),
(3,'Constracion',2200.00),
(2,'Vacunacion ovina',850.00),
(1,'Revision general',550.00),
(3,'Supervision de ganados',800.00)

select M.especie,SUM (C.costo) as CostoTotalConsultas
from Consultas C
join Mascotas M on C.id_mascotas= M.id_mascotas
group by M. especie

select M.especie,SUM (C.costo) as CostoTotalConsultas
from Consultas C, Mascotas m
where c.id_mascotas = m.id_mascotas
group by M.especie

select M.nombre,AVG(C.costo) as Promedio_Consulta
from Consultas C
JOIN Mascotas M on C.id_mascotas = M.id_mascotas
group by M.nombre

-- Costo total y promedio de consultas por mascotas
SELECT 
    M.nombre,
    SUM(c.costo) AS costo_total,
    AVG(c.costo) AS costo_promedio,
    MAX(c.costo) AS maximo,
    MIN(c.costo) AS minimo
FROM Consultas C
JOIN Mascotas m on C.id_mascotas = M.id_mascotas
GROUP BY m.nombre

-- costo mas caro y mas barato por mascota
SELECT 
    M.nombre,
    MAX(c.costo) AS costo_total,
    MIN(c.costo) AS costo_promedio
FROM consultas C
JOIN Mascotas m on C.id_mascotas = M.id_mascotas
GROUP BY m.nombre

-- Dueños con gastos total >$1000
SELECT 
    m.id_dueño,
    m.id_mascotas,
    SUM(c.costo) AS gasto_total
FROM Consultas c
JOIN Mascotas M ON c.id_mascotas = m.id_mascotas
JOIN dueño d ON m.id_dueño = d.id_dueño
GROUP BY m.id_dueño
HAVING SUM(c.costo) > 1000;

