---
title: "Logging activity"
description: "Learn how to configure different combinations of logging options when using the Microsoft Drivers for PHP for SQL Server"
author: David-Engel
ms.author: v-davidengel
ms.date: "09/22/2020"
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
  - "logging activity"
---
# Logging Activity
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

By default, errors and warnings that are generated by the [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] are not logged to the PHP system log. This topic discusses how to configure driver logging activity. For more information on how to configure PHP error handling settings that are not specific to the drivers, see the [PHP documentation](https://www.php.net/manual/en/errorfunc.configuration.php).  
  
## Logging Activity Using the PDO_SQLSRV Driver  
The only available logging configuration specific to the PDO_SQLSRV driver is the pdo_sqlsrv.log_severity entry in the php.ini file.  
  
Add the following at the end of your php.ini file:  
  
```  
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = <number>  
```  
  
**log_severity** can be one of the following values:  
  
|Value|Description|  
|---------|---------------|  
|0|Logging is disabled (is the default if nothing is defined).|  
|-1|Specifies that errors, warnings, and notices are logged.|  
|1|Specifies that errors are logged.|  
|2|Specifies that warnings are logged.|  
|4|Specifies that notices are logged.|  
  
Logging information is added to the phperrors.log file.  
  
PHP reads the configuration file on initialization and stores the data in a cache; it also provides an API to update these settings and use right away, and is written to the configuration file. This API enables application scripts to change the settings even after PHP initialization.  
  
## Logging Activity Using the SQLSRV Driver  
To turn on logging, you can use the [sqlsrv_configure](../../connect/php/sqlsrv-configure.md) function or you can alter the php.ini file. You can log activity on initializations, connections, statements, or error functions. You can also specify whether to log errors, warnings, notices, or all three.  
  
> [!NOTE]  
> You can configure the location of the log file in the php.ini file. See the [PHP documentation](https://www.php.net/manual/en/errorfunc.configuration.php) for more details.  
  
### Turning Logging On  
You can turn on logging by using the [sqlsrv_configure](../../connect/php/sqlsrv-configure.md) function to specify a value for the **LogSubsystems** setting. For example, the following line of code configures the driver to log activity on connections:  
  
`sqlsrv_configure("LogSubsystems", SQLSRV_LOG_SYSTEM_CONN);`  
  
The following table describes the constants that can be used as the value for the **LogSubsystems** setting:  
  
|Value (integer equivalent in parentheses)|Description|  
|-----------------------------------------------|---------------|  
|SQLSRV_LOG_SYSTEM_ALL (-1)|Turns on logging of all subsystems.|  
|SQLSRV_LOG_SYSTEM_OFF (0)|Turns logging off. This is the default.|  
|SQLSRV_LOG_SYSTEM_INIT (1)|Turns on logging of initialization activity.|  
|SQLSRV_LOG_SYSTEM_CONN (2)|Turns on logging of connection activity.|  
|SQLSRV_LOG_SYSTEM_STMT (4)|Turns on logging of statement activity.|  
|SQLSRV_LOG_SYSTEM_UTIL (8)|Turns on logging of error functions activity (such as handle_error and handle_warning).|  
  
You can set more than one value at a time for the **LogSubsystems** setting by using the logical OR operator (|). For example, the following line of code turns on logging of activity on both connections and statements:  
  
`sqlsrv_configure("LogSubsystems", SQLSRV_LOG_SYSTEM_CONN | SQLSRV_LOG_SYSTEM_STMT);`  
  
You can also turn on logging by specifying an integer value for the **LogSubsystems** setting in the php.ini file. For example, adding the following line to the `[sqlsrv]` section of the php.ini file turns on logging of connection activity:  
  
`sqlsrv.LogSubsystems = 2`  
  
By adding integer values together, you can specify more than one option at a time. For example, adding the following line to the `[sqlsrv]` section of the php.ini file turns on logging of connection and statement activity:  
  
`sqlsrv.LogSubsystems = 6`  
  
### Logging Errors, Warnings, and Notices  
After turning logging on, you must specify what to log. You can log one or more of the following: errors, warnings, and notices. For example, the following line of code specifies that only warnings are logged:  
  
`sqlsrv_configure("LogSeverity", SQLSRV_LOG_SEVERITY_WARNING);`  
  
> [!NOTE]  
> The default setting for **LogSeverity** is **SQLSRV_LOG_SEVERITY_ERROR**. If logging is turned on and no setting for **LogSeverity** is specified, only errors are logged.  
  
The following table describes the constants that can be used as the value for the **LogSeverity** setting:  
  
|Value (integer equivalent in parentheses)|Description|  
|-----------------------------------------------|---------------|  
|SQLSRV_LOG_SEVERITY_ALL (-1)|Specifies that errors, warnings, and notices are logged.|  
|SQLSRV_LOG_SEVERITY_ERROR (1)|Specifies that errors are logged. This is the default.|  
|SQLSRV_LOG_SEVERITY_WARNING (2)|Specifies that warnings are logged.|  
|SQLSRV_LOG_SEVERITY_NOTICE (4)|Specifies that notices are logged.|  
  
You can set more than one value at a time for the **LogSeverity** setting by using the logical OR operator (|). For example, the following line of code specifies that errors and warnings should be logged:  
  
`sqlsrv_configure("LogSeverity", SQLSRV_LOG_SEVERITY_ERROR | SQLSRV_LOG_SEVERITY_WARNING);`  
  
> [!NOTE]  
> Specifying a value for the **LogSeverity** setting does not turn on logging. You must turn on logging by specifying a value for the **LogSubsystems** setting, then specify the severity of what is logged by setting a value for **LogSeverity**.  
  
You can also specify a setting for the **LogSeverity** setting by using integer values in the php.ini file. For example, adding the following line to the `[sqlsrv]` section of the php.ini file enables logging of warnings only:  
  
`sqlsrv.LogSeverity = 2`  
  
By adding integer values together, you can specify more than one option at a time. For example, adding the following line to the `[sqlsrv]` section of the php.ini file enables logging of errors and warnings:  
  
`sqlsrv.LogSeverity = 3`  
  
## See Also  
[Programming Guide for the Microsoft Drivers for PHP for SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Constants &#40;Microsoft Drivers for PHP for SQL Server&#41;](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)

[sqlsrv_configure](../../connect/php/sqlsrv-configure.md)

[sqlsrv_get_config](../../connect/php/sqlsrv-get-config.md)

[SQLSRV Driver API Reference](../../connect/php/sqlsrv-driver-api-reference.md)  
  
