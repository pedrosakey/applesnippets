

Deconstruir práctica 2 para poder reautilizar elementos.


 ➡TimeLine
⋙ Next Actions 
✔️  Revisar commit a commit y anotaciones
✔️ Prototype. Podemos hacer interesante prototipos sólo con el storyboard sin necesidad de codificar nada

✔️  Download Fake Implementation

Example of architecture


Interactors. Between layers

import Foundation

protocol DownloadAllShopsInteractor {
    func execute(onSuccess: @escaping (Shops) -> Void, onError: errorClosure)
    func execute(onSuccess: @escaping (Shops) -> Void)
}
 
Example of implementation:

class DownloadAllShopsInteractorFakeImpl: DownloadAllShopsInteractor {
    func execute(onSuccess: @escaping (Shops) -> Void) {
        execute(onSuccess: onSuccess, onError: nil)
    }
    
    func execute(onSuccess: @escaping (Shops) -> Void, onError: errorClosure = nil) {
        let shops = Shops()
        
        for i in 0...10 {
            let shop = Shop(name: "Shop number \( i )")
            shop.address = "Address \( 1 )"
            
            shops.add(shop: shop)
        }
        
        OperationQueue.main.addOperation {
            onSuccess(shops)
        }
    }
    
    
}

Example of use

import UIKit

class ShopsViewController: UIViewController {
    
    var shops: Shops?
   
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let downloadShopsInteractor: DownloadAllShopsInteractor = DownloadAllShopsInteractorFakeImpl()
        
        downloadShopsInteractor.execute { (shops: Shops) in
            // todo OK
            for i in 1...10 {
            print("Name: " + shops.get(index: i).name)
            }
            }
        

    }

}

📝Notes
Comparar esta arquuitetura clean code con viper
Una vez implementado el interactor podemos obtener las tiendas de distintas fuenes, fake, red o db com cambio mínimo en la implementación.

 ✔️ DowloadAllShopsInteractorFakeImpl & show in CollectionView
Este commit no cumple lo que promete ya que solo pinta las shops en consola.

✔️ DowloadAllShopsInteractorNSDataImp
import Foundation

class DownloadAllShopsInteractorNSDataImpl : DownloadAllShopsInteractor {
    func execute(onSuccess: @escaping (Shops) -> Void) {
        execute(onSuccess: onSuccess, onError: nil)
    }
    
    func execute(onSuccess: @escaping (Shops) -> Void, onError: errorClosure = nil) {
        
        let urlString = "https://madrid-shops.com/json_new/getShops.php"
        
        let queue = OperationQueue()
        queue.addOperation {
            
            if let url = URL(string: urlString), let data = NSData(contentsOf: url) as Data? {
                do {
                    let jsonObject = try JSONSerialization.jsonObject(with: data, options: JSONSerialization.ReadingOptions.allowFragments) as! Dictionary<String, Any>
                    let result = jsonObject["result"] as! [Dictionary<String, Any>]
                    
                    let shops = Shops()
                    for shopJson in result {
                        let shop = Shop(name: shopJson["name"]! as! String)
                        shop.address = shopJson["address"]! as! String
                        
                        shops.add(shop: shop)
                    }
                    
                    OperationQueue.main.addOperation {
                        onSuccess(shops)
                    }
                } catch {
                    // Error
                }
            }
        }
        
        
    }
    
   
}

✔️ DownloadAllShopsInteractorNSURLSession

import Foundation

class DownloadAllShopsInteractorNSURLSessionImpl: DownloadAllShopsInteractor {
    func execute(onSuccess: @escaping (Shops) -> Void, onError: errorClosure) {
        let urlString = "https://madrid-shops.com/json_new/getShops.php"
        
        let session = URLSession.shared
        if let url = URL(string: urlString) {
            let task = session.dataTask(with: url) { (data: Data?, response: URLResponse?, error: Error?) in
                
                OperationQueue.main.addOperation {
                    assert(Thread.current == Thread.main)
                    
                    if error == nil {
                        let shops = parseShops(data: data!)
                        onSuccess(shops)
                    } else {
                        if let myError = onError {
                            myError(error!)
                        }
                    }
                }
            }
            task.resume()
        }
    }
    
    func execute(onSuccess: @escaping (Shops) -> Void) {
        execute(onSuccess: onSuccess, onError: nil)
    }
    
    
}

✔️ Basic AutoLayout MainVC & ShopsVC
Guia completa de Autolayout

✔️ ShopDetail desing + Segue …
✔️ Trigger manual Segue

Guia completa Segue

✔️ Core Data

Guía completa CoreData

✔️ Fix - ShopsVewController. Wrong Interactor :S
✔️ Core Data - Fetch …
✔️ ExecuteOnce & Fix segue
✔️ Pin Shops in MapView - ShopsViewController …
import MapKit
import CoreLocation

       //Request Authorizacion
        self.locationManager.requestWhenInUseAuthorization()
        
        //Pin a Map
        self.map.delegate = self
        
        
        //Location latitude & longitude
        let madridLocation = CLLocation(latitude: 40.416775, longitude: -3.703790)
        self.map.setCenter(madridLocation.coordinate, animated: true)
        
        //zoom
        let region = MKCoordinateRegion(center: madridLocation.coordinate, span: MKCoordinateSpan(latitudeDelta: 1, longitudeDelta: 1))
        let reg = self.map.regionThatFits(region)
        self.map.setRegion(reg, animated: true)
        
        
        //Add anotation
        let mapPin = MapPin(coordinate: madridLocation.coordinate)
        mapPin.title = "titutlo"
        mapPin.subtitle = "subtittitio"
        
        self.map.addAnnotation(mapPin)

⋮

✔️ REFACTORING - Pin Shops in MapView - ShopsViewController

✔️ Show error without connection

✔️ Image Cache through CocoaPod
Guía Cocoapod

✔️ Activities - Full activities functionalities

✔️ Download Activities


✔️ Activity Prototype


✔️ English (Base) & Spanish with Strings Localization

Guia de internacionalizacion COLLOC 



➡ Live Productivity

➡ Block 


➡ NoteStack

Conclusiones
No se puede deconstruir bien esta app porque esta todo muy acoplado.
Hacer las guias propuestas y añadirlas a la documentación


