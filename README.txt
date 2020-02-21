Структура Airsim:
1) Основной исполняемый проект Unreal/Enviroments/Blocks/
2) Проект в виде плагина туда закачивается в Plugins/AirSim/Source/AirLib/
3) Данные обрабатываются через Plugins\AirSim\Source\AirLib\include\vehicles\multirotor\firmwares\mavlink\MavLinkMultirotorApi.hpp

Функция входящих сообщений: void processMavMessages(const mavlinkcom::MavLinkMessage& msg)
Получение данных 
virtual real_T getActuation(unsigned int rotor_index) const override
Функция исходящих сообщений: 
void sendHILGps(const GeoPoint& geo_point, const Vector3r& velocity, float velocity_xy, float cog,
        float eph, float epv, int fix_type, unsigned int satellites_visible)
void sendHILSensor(const Vector3r& acceleration, const Vector3r& gyro, const Vector3r& mag, float abs_pressure, float pressure_alt) 

4) Путь к базовому классу транспорта \AirSim\Unreal\Environments\Blocks\Plugins\AirSim\Source\AirLib\include\api\VehicleApiBase.hpp

5) 


//Physics engine calls this method to set next kinematics

class MultiRotor : public PhysicsBody {
...

    virtual void updateKinematics(const Kinematics::State& kinematics) override
    {
    ...
    		vehicle_api_->getActuation(rotor_index));
    ...
}}

in file \AirSim\Unreal\Environments\Blocks\Plugins\AirSim\Source\AirLib\include\physics\FastPhysicsEngine.hpp
     void updatePhysics(PhysicsBody& body)
     {
     ...

     	body.setWrench(next_wrench);
        body.updateKinematics(next);

     ...
     }

MultiRotor.hpp -> MultiRotorPawnSimApi.h -> SimModeWorldMultiRotor.h ( ASimModeWorldMultiRotor : public ASimModeWorldBase : public ASimModeBase) -> WorldSimApi.h

AirSim\AirLib -- это основные моменты физики и управления

AirSim\Unreal\Plugins\AirSim\Source -- это имплементация объекта в анриал

AirSim\Unreal\Environments\Blocks -- экспериментальная площадка для симуляции по умолчанию. Ее обновляет команда update_from_git.bat. Сборка проекта идет через .sln
