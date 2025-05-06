import java.util.regex.Pattern

class PasswordMismatchException(message: String) : Exception(message)

fun registerUser(login: String?, password1: String?, password2: String?): String {
    // Step 2: Replace null login or password with default values
    val userLogin = login ?: "guest"
    val userPassword1 = password1 ?: "12345678"
    val userPassword2 = password2 ?: "12345678"

    // Step 3: Check if passwords match
    if (userPassword1 != userPassword2) {
        throw PasswordMismatchException("Паролі не збігаються.")
    }

    // Step 4: Check password complexity
    if (userPassword1.length < 8) {
        return "Пароль повинен містити не менше 8 символів."
    }
    if (!Pattern.compile(".*\\d.*").matcher(userPassword1).matches()) {
        return "Пароль повинен містити хоча б одну цифру."
    }
    if (!Pattern.compile(".*[A-Z].*").matcher(userPassword1).matches()) {
        return "Пароль повинен містити хоча б одну велику літеру."
    }

    // Step 5: Registration successful
    return "Користувача зареєстровано (логін: $userLogin)"
}

fun main() {
    print("Введіть логін: ")
    val login = readLine()

    print("Введіть пароль: ")
    val password1 = readLine()

    print("Повторіть пароль: ")
    val password2 = readLine()

    try {
        val result = registerUser(login, password1, password2)
        println(result)
    } catch (e: PasswordMismatchException) {
        println("Помилка: ${e.message}")
    }
}
