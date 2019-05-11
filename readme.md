# Apple Snippets

![](https://drive.google.com/uc?id=1_KK9LXPwgyMrcCfvS9JFEeErp55tJ7NN)

**[Obj C]** 

A collection of snippets relate with apple development. 

// Repository Example

```

import Foundation

final class Repository{
    
    static let local = LocalFactory()
}

protocol HouseFactory {
    
    var houses : [House] {get}
}

final class LocalFactory : HouseFactory {
    var houses: [House]{
        get {
            //Create houses
          ...
                
            // Add characters
          ...
      
            return [stark, lannister].sorted()
        }
    }
    
}

```