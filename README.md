# Challenge Rappi


## Código versión inicial

```php

public function post_confirm(){
    $id = Input::get('service_id');
    $servicio = Service::find($id);
    //dd($Servicio);
    if ($servicio != NULL){
        if  ($servicio->status_id == '6'){
            return Response::json(array('error' => '2'));
        }
        if ($servicio->driver_id == NULL && $servicio->status_id == '1'){
            $servicio = Service::update($id, array(
                'driver_id' => Input::get('driver_id'),
                'status_id' => '2'
                //Up Carro
                //,'pwd' => md5(Input::get('pwd'))
            ));
            Driver::update(Input::get('driver_id'), array(
                'available' => '0'
            ));
            $driverTmp = Driver::find(Input::get('driver_id'));
            Service::update($id, array(
                'car_id'=>$driverTmp->car_id
                //Up Carro
                //,'pwd' => md5(Input::get('pwd'))
            ));
            //Notificar a usuario!!
            $pushMessage = 'Tu servicio ha sido confirmado!';
            /*$servicio = Service::find($id);
            $push = Push::make();
            if ($servicio->user->type=='1'){//iPhone
                $pushAns = $push->ios($servicio->user->uuid, $pushMessage);
            }else{
                $pushAns = $push->android($servicio->user->uuid, $pushMessage);
            }*/
            $servicio = Service::find($id);
            $push = Push::make();
            if ($servicio->user->uuid == ''){
                return Response::json(array('error'=>'0'));
            }
            if($servicio->user->type == '1'){//iPhone
                $result = $push->ios($servicio->user->uuid, $pushMessage, 1, 'honk.wav',
                                        'Open', array('service_id'=>$servicio->id));
            } else{
                $result = $push->android2($servicio->user->uuid, $pushMessage, 1, 'default',
                                        'Open', array('service_id'=>$servicio->id));
            }
            return Response::json(array('error'=>'0'));
        }else{
            return Response::json(array('error'=>'1'));
        }
    }else{
        return Response::json(array('error'=>'3'));
    }
}
```

## Código versión corregida
```php
const ERROR = 'Error';
const ERROR_INVALID_REQUEST =3
const ERROR_PARAMETERS_CONFLICT = 2
const ERROR_PARAMETERS_CONFLICT = 2
const ERROR_DRIVER = 1


public function post_confirm(){
$service_id = Input::get('service_id');
$service = Service::find($service_id);
if ($service != NULL) {
    if ($service->status_id == '6') {
        return Response::json(array(ERROR => ERROR_PARAMETERS_CONFLICT));
    }
    if ($service->driver_id == NULL && $service->status_id == '1')
    {
            $driver_id = Input::get('driver_id');
            $driverTmp = Driver::find($driver_id);
            $servicio = Service::update($service_id, array(
                'driver_id' => $driver_id,
                'status_id' => '2',
                car_id' => $driverTmp->car_id
            ));
            Driver::update($driver_id, array(
            'available' => '0'
            ));	
            //Notificar a usuario!!
            $pushMessage = 'Tu servicio ha sido confirmado!';
            $push = Push::make();
            if ($service->user->type == '1') {//iPhone
                $push->ios($servicio->user->uuid, $pushMessage, 1, 'honk.wav', 'Open', array('service_id' => $servicio->id));
            }
            else{
                $push->android2($servicio->user->uuid, $pushMessage, 1, 'default', 'Open', array('service_id' => $servicio>id));
            }
        }
            return Response::json(array(ERROR => '0'));
    }
    else{
            return Response::json(array(ERROR=> ERROR_DRIVER));
        }
}
else{
        return Response::json(array(ERROR=> ERROR_INVALID_REQUEST));
    }
}
```

## Argumentación
- Se recomienda que los nombres de las variabled sean dicientes, es decir que reflejen el contenido, dado que en el refactor de codigo se encuentran variables que son nombradas como "id", que puede hacer referencia a cualquier id, y no beneficia la mantenibilidad del software

 - Debe existir un standard de codigo que brinde las herramientas de nombramiento de variables, ya que en el codigo se encuentran variables en ingles, variables en español 
 
 - Los comentarios en codigo son muy utilizados en cuanto aporte a la mantenibilidad del software y pueda servir de ayuda a futuros desarrolladores para comprender la funcionalidad del codigo, a la vez tambien pueden servir de ayuda para el desarrollador al establecer "ToDo" que son comentarios de codigo de funcionalidades esperadas que no se han terminado, sin emabargo en el codigo presentado se evidencian partes de codigo que estan comentadas, que no aportan un valor significativo a los desarrolladores
 
 - En ocasiones es necesario declarar constantes con valores string que permitan utilizarlas en las diferentes partes del codigo, por ejemplo en varias partes del codigo se utiliza el string 'Error', lo que se sugiere es que como se trata de un concepto de error tendra que ser generico por tal motivo se decide declarar unc constante con dicho valor, con el fin de que si se desea cambiar solo se deba hacer la modificación a la constante declarada
 
 - Se evidencia que en el codigo se devuelve un json "key" "valor", en ciertas ocasiones sin embargo por temas de mantenibilidad se recomienda que dichos valores se cambien por constantes que sean lo suficientemente dicientes del error ocurrido, por ejemplo el error 3 representa que no se recibieron parametros en la petición por lo que se puede reemplazar por una constante llamada ERROR_INVALID_REQUEST que tenga el valor de 3
 
-  Se evidencia que en codigo a analizar se esta llamando dos veces al metodo update, en el primer caso envia dos parametros $driver_id y $status_id, y en el segundo caso envia car_id, por lo que se sugiere unificar los metodos mencionados en el que solo se llame una unica vez con los tres parametros
 
 



