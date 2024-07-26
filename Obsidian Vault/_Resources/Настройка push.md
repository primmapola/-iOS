Давайте рассмотрим, как настроить и использовать различные типы push-уведомлений в iOS на примере Swift кода. Для этого нам потребуется:

1. Запросить разрешение у пользователя на получение уведомлений.
2. Зарегистрировать типы уведомлений, которые приложение собирается использовать.
3. Отправить уведомление с сервера, используя APNs.

### Шаг 1: Запрос разрешения

Для начала, нам нужно импортировать необходимые модули и запросить разрешение у пользователя:

```swift
import UIKit
import UserNotifications

class AppDelegate: UIResponder, UIApplicationDelegate {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Запрос разрешения на получение уведомлений
        let center = UNUserNotificationCenter.current()
        center.requestAuthorization(options: [.alert, .sound, .badge]) { granted, error in
            if granted {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            } else {
                print("Permission denied")
            }
        }
        return true
    }

    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        // Преобразование и отправка токена устройства на ваш сервер
        let tokenParts = deviceToken.map { data in String(format: "%02.2hhx", data) }
        let token = tokenParts.joined()
        print("Device Token: \(token)")
    }

    func application(_ application: UIApplication, didFailToRegisterForRemoteNotificationsWithError error: Error) {
        print("Failed to register: \(error)")
    }
}
```

### Шаг 2: Обработка уведомлений

Код для обработки входящих уведомлений:

```swift
func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    // Показать уведомление, даже если приложение открыто
    completionHandler([.alert, .badge, .sound])
}

func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    // Действия при нажатии на уведомление
    completionHandler()
}
```

### Шаг 3: Отправка уведомления через APNs

Отправка уведомления осуществляется через сервер, который должен использовать корректный сертификат для аутентификации в APNs и отправить JSON пакет, который может выглядеть примерно так:

```json
{
    "aps": {
        "alert": "Hello, world!",
        "sound": "default",
        "badge": 1
    }
}
```

Это основы настройки push-уведомлений в iOS приложении. Конечно, для реальной работы вашего приложения потребуется настроить серверную часть, которая будет отправлять уведомления через APNs, а также обеспечить безопасное хранение и использование токенов устройств.