# TwitterLBTA
Twitter Home screen in code using some Thirdparties 
(1) LBTAComponents
      this library is best for some its components 
      for ..... 
        collectionview in code for Different components Model
        it have including its anchor method for giving constraints
        it have superView method for adding view on full screen using constraints
        
        
(2) Network library TRON with swifty JSON
    
        import Foundation
        import TRON
        import SwiftyJSON

        struct Service {

            let tron = TRON(baseURL: "https://api.letsbuildthatapp.com")

            static let sharedInstance = Service()

            func fetchHomeFeed(completion: @escaping (HomeDatasource?, Error?) -> ()) {

                let request: APIRequest<HomeDatasource, JSONError> = tron.request("/twitter/home")

                request.perform(withSuccess: { (homeDatasource) in
                    completion(homeDatasource, nil)
                }) { (err) in

                    completion(nil, err)
                }
            }

            class JSONError: JSONDecodable {
                required init(json: JSON) throws {
                    print("JSON ERROR")
                }
            }

        }
 
(3)  Generic function to map array that confirm protocol JSON 
     so can parse array but model class must be confirm JSONDecodable Protocol
     
        extension Collection where Iterator.Element == JSON {
         func decode<T: JSONDecodable>() throws -> [T] {
          return try map{try T(json: $0)}
         }
        }



(4) NSError in tron API 

        required init(json: JSON) throws {
        guard let usersJsonArray = json["users"].array, let tweetsJsonArray = json["tweets"].array else {
            throw NSError(domain: "com.letsbuildthatapp", code: 1, userInfo: [NSLocalizedDescriptionKey: "Parsing JSON was not valid."])
        }
        self.users = try usersJsonArray.decode()
        self.tweets = try tweetsJsonArray.decode()
        
        }
        
