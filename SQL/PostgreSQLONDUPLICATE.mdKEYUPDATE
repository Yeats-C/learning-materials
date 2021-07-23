# PostgreSQL ON DUPLICATE KEY UPDATE

* on conflict 功能，例如：

```
INSERT INTO "public"."basic_gx" (
	"geom",
	"gx_name",
	"gx_code",
	"key",
	"ascription_enterprise",
	"ascription_enterprise_code",
	"province",
	"city",
	"gx_type",
	"pressure_level",
	"gx_length",
	"gx_diameter",
	"design_pressure",
	"design_capacity",
	"gas_source",
	"initial_station_type",
	"initial_station_name",
	"terminal_station_type",
	"terminal_station_name",
	"route_area",
	"production_date",
	"state" 
) SELECT
"geom",
"gx_name",
"gx_code",
"key",
"ascription" AS ascription_enterprise,
"ascripti_1" AS ascription_enterprise_code,
"province",
"city",
"gx_type",
"pressure_l" AS pressure_level,
"gx_length",
"gx_diamete"as gx_diameter,
"design_pre" AS design_pressure,
"design_cap" AS design_capacity,
"gas_source",
"initial_st" AS initial_station_type,
"initial__1" AS initial_station_name,
"terminal_s" AS terminal_station_type,
"terminal_1" AS terminal_station_name,
"route_area",
"production" AS production_date,
"state" 
FROM "public"."国家管线20210715" 

-- ON DUPLICATE KEY UPDATE  
ON CONFLICT (gx_name,gx_code) DO UPDATE SET 
		geom=excluded.geom,
		key=excluded.key,
		ascription_enterprise=excluded.ascription_enterprise,
		ascription_enterprise_code=excluded.ascription_enterprise_code,
		province=excluded.province,
		city=excluded.city,
		gx_type=excluded.gx_type,
		pressure_level=excluded.pressure_level,
		gx_length=excluded.gx_length,
		gx_diameter=excluded.gx_diameter,
		design_pressure=excluded.design_pressure,
		design_capacity=excluded.design_capacity,
		gas_source=excluded.gas_source,
		initial_station_type=excluded.initial_station_type,
		initial_station_name=excluded.initial_station_name,
		terminal_station_type=excluded.terminal_station_type,
		terminal_station_name=excluded.terminal_station_name,
		route_area=excluded.route_area,
		production_date=excluded.production_date,
		state=excluded.state,
		update_time=NOW();
```

