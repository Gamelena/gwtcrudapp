# Setup #
  1. Checkout the code
  1. Setup the database
  1. Run the project

## Checkout the code ##
This project is developed based on IntelliJ IDEA Community Edition, First checkout the code from the Google code repository using a SVN Client. Once project is checked out the

## Setup the database ##
The project is using a MySQL database, and the database configuration has to be setup in the /conf/config.properties file in the loaderweb project.

```
database_name=bookingengine
database_pass=
database_user=root
database_port=3306
database_ip=127.0.0.1
```

apply the following script to setup the database. Alternatively you can execute the function InstallUtil.patchDB against the database so that it will setup the database.

```
delimiter $$

CREATE TABLE `auth_user` (
  `user_id` int(11) NOT NULL,
  `entity_Id` int(11) NOT NULL,
  `version` int(11) NOT NULL,
  `user_Name` varchar(100) NOT NULL,
  `password` varchar(255) NOT NULL,
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_config` (
  `config_name` varchar(255) NOT NULL,
  `config_value` varchar(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_entity` (
  `entity_id` int(11) NOT NULL,
  `entity_type_id` int(11) NOT NULL,
  PRIMARY KEY (`entity_id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_entity_type` (
  `entity_type_id` int(11) NOT NULL,
  `entity_Type_Code` varchar(20) NOT NULL,
  PRIMARY KEY (`entity_type_id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_entity_property` (
  `entity_id` int(11) NOT NULL,
  `entity_type_id` int(11) NOT NULL,
  `property_type_id` int(11) NOT NULL,
  `property_Value` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`entity_id`,`entity_type_id`,`property_type_id`),
  CONSTRAINT `fk_entity2entity_property` FOREIGN KEY (`entity_id`) REFERENCES `gen_entity` (`entity_id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_entity_property_type` (
  `entity_type_id` int(11) NOT NULL,
  `property_type_id` int(11) NOT NULL,
  `property_Code` varchar(20) NOT NULL,
  `property_Description` varchar(20) NOT NULL,
  `property_Type` varchar(20) NOT NULL,
  `default_Value` varchar(20) NOT NULL,
  PRIMARY KEY (`entity_type_id`,`property_type_id`),
  CONSTRAINT `fk_entity_type2entity_property` FOREIGN KEY (`entity_type_id`) REFERENCES `gen_entity_type` (`entity_type_id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

CREATE TABLE `gen_sequence` (
  `sequence_id` varchar(255) NOT NULL,
  `next_value` varchar(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1$$

INSERT INTO `auth_user`
(`user_id`,
`entity_Id`,
`version`,
`user_Name`,
`password`)
VALUES
(1,1,0,'root','asd')$$

INSERT INTO `gen_sequence`
(`sequence_id`,
`next_value`)
VALUES
('gen_entity',10)$$

INSERT INTO `gen_sequence`
(`sequence_id`,
`next_value`)
VALUES
('auth_user',10)$$

INSERT INTO `gen_entity`
(`entity_id`,
`entity_type_id`)
VALUES
(1,0)$$
```

change the settings to match your database.

## Run the project ##
Their is a ant script ('build.xml') included to run the application, with debug mode. The IntelliJ IDEA project is already setup to debug the application. First run the target build in the ant project. After than run the target 'devmode'. Once it's listening to the port execute the IntelliJ IDEA configuration named 'Remote'. This should bring up the GWT Development Mode window. And from their you can use the URL to paste onto the browser(which need to be installed GWT plug-in). And use the user name 'root' with password 'asd' to logon to the system.