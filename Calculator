import java.math.BigInteger

fun  newCalculateSum(str: String) {
    val temp = str
    val result = mutableListOf<String>()
    val stack = mutableListOf<String>()
    val secondStack = mutableListOf<BigInteger>()
    val pattern = "[*/][*/]+".toRegex()
    val mainPattern = "(^-)?\\d+|\\+|-|\\(|\\)|\\*|/".toRegex()
    val sortedTemp = mainPattern.findAll(temp)

    if (sortedTemp.count { it.value == "(" } != sortedTemp.count { it.value == ")" }) {
        println("Invalid expression")
        return
    }
    if (pattern.containsMatchIn(temp)) {
        println("Invalid expression")
        return
    }

    // converting from infix to postfix

    sortedTemp.forEach {
        when (it.value) {
            "(" -> stack.add("(")
            ")" -> {
                while (stack.isNotEmpty() && stack.last() != "(") {
                    result.add(stack.last())
                    stack.removeLast()
                }
                stack.removeLast()
            }
            "+" -> {
                if (stack.isEmpty() || stack.last() == "(") stack.add("+")
                else {
                    do {
                        result.add(stack.last())
                        stack.removeLast()
                        if (stack.isEmpty() || stack.last() == "(") break
                    } while (stack.last() == "*" || stack.last() == "/" || stack.last() == "(")
                    stack.add("+")
                }
            }
            "-" -> {
                if (stack.isEmpty() || stack.last() == "(") stack.add("-")
                else {
                    do {
                        result.add(stack.last())
                        stack.removeLast()
                        if (stack.isEmpty() || stack.last() == "(") break
                    } while (stack.last() == "*" || stack.last() == "/" || stack.last() == "(")
                    stack.add("-")
                }
            }
            "*" -> {
                if (stack.isEmpty() || stack.last() == "(") stack.add("*")
                else if (stack.last() == "*" || stack.last() == "/") {
                    result.add(stack.last())
                    stack.removeLast()
                    stack.add("*")
                } else stack.add("*")
            }
            "/" -> {
                if (stack.isEmpty() || stack.last() == "(") stack.add("/")
                else if (stack.last() == "*" || stack.last() == "/") {
                    result.add(stack.last())
                    stack.removeLast()
                    stack.add("/")
                } else stack.add("/")
            }

            else -> {
                result.add(it.value)
            }
        }
    }
    for (item in stack.asReversed()) {
        result.add(item)
    }

    // calculating the result

    for (item in result) {
        when(item) {
            // if operator pop twice, perform operation, push result to stack
            "+" -> {
                val twoNumbers = secondStack.last() + secondStack[secondStack.lastIndex - 1]
                repeat(2) {
                    secondStack.removeLast()
                }
                secondStack.add(twoNumbers)
            }
            "-" -> {
                val twoNumbers = secondStack[secondStack.lastIndex - 1] - secondStack.last()
                repeat(2) {
                    secondStack.removeLast()
                }
                secondStack.add(twoNumbers)
            }
            "*" -> {
                val twoNumbers = secondStack.last() * secondStack[secondStack.lastIndex - 1]
                repeat(2) {
                    secondStack.removeLast()
                }
                secondStack.add(twoNumbers)
            }
            "/" -> {
                val twoNumbers = secondStack[secondStack.lastIndex - 1] / secondStack.last()
                repeat(2) {
                    secondStack.removeLast()
                }
                secondStack.add(twoNumbers)
            }
            else -> secondStack.add(item.toBigInteger())
        }
    }
    println(secondStack.last())
}

fun prepCalculateSum (str: String): String {
    var temp = str.replace(" ", "")
    val pattern = "(\\+-)|(-\\+)|(--)|(\\+\\+)".toRegex()
    while (temp.contains(pattern)) {
        pattern.findAll(temp).forEach {
            when(it.value) {
                "+-" -> temp = temp.replace("+-", "-")
                "-+" -> temp = temp.replace("-+", "-")
                "--" -> temp = temp.replace("--", "+")
                "++" -> temp = temp.replace("++", "+")
            }
        }
    }
    return temp
}

fun checkValidity (string: String): Boolean {
    val pattern = "(\\D*(\\d+\\s+\\d+\\D?)*)|(\\d+[+-]+)".toRegex()
    return pattern.matches(string)
}

fun isVariableValid (string: String): Int? {
    val patternInvalidIdentifier = "^(([a-zA-Z]+\\d+)|(\\d+[a-zA-Z]+)|([^\\s\\d/()\\-+a-zA-Z]+))".toRegex()
    val patternSingleVariable = "[a-zA-Z]+".toRegex()
    val patternDoubleVariable = "\\s*[a-zA-Z]+\\s*=\\s*[a-zA-Z]+\\s*".toRegex()
    val patternNewAssignment = "\\s*[a-zA-Z]+\\s*=\\s*(-)?\\s*\\d+\\s*".toRegex()
    val patternInvalidAssignment = "[a-zA-Z]+\\s*=\\s*(([a-zA-Z]+\\d+)|(\\d+[a-zA-Z]+)|([\\w\\s\\-+]+=))".toRegex()

    when  {
        patternInvalidIdentifier.containsMatchIn(string) -> return 1
        patternInvalidAssignment.containsMatchIn(string) -> return 5
        patternDoubleVariable.matches(string) -> return 3 // a variable's value is assigned to a new variable, or error
        patternNewAssignment.matches(string) -> return 4 // variable is assigned a value, or error
        patternSingleVariable.matches(string) -> return 2
    }
    return null
}

fun convertLetters(str: String, map: MutableMap<String, BigInteger>): String {
    var temp2 = str.replace("=", "") // not sure the replacement is needed, re-check this part
    val pattern = "[a-zA-Z]+".toRegex()
    pattern.findAll(temp2).forEach {
        if (map.containsKey(it.value)) {
            temp2 = temp2.replace(it.value, map.getValue(it.value).toString())
        }
    }
    return temp2
}

fun main() {

    val storesVariables = mutableMapOf<String, BigInteger>()
    while (true) {

        when (val equation = readln()) {
            "/exit" -> return println("Bye!")
            // terminates program.
            "/help" -> println("The program calculates the sum of numbers")
            "" -> continue
            else -> {
                when (isVariableValid(equation)) {
                    1 -> {
                        println("Invalid identifier")
                        continue
                    }
                    2 -> {
                        if (storesVariables.containsKey(equation)) println(storesVariables[equation]) else println("Unknown variable")
                        continue
                    }
                    3 -> {
                        val tempList = equation.replace(" ", "").split("=").toList()
                        if (storesVariables.containsKey(tempList[1])) {
                            val temp = storesVariables.getValue(tempList[1])
                            storesVariables[tempList[0]] = temp
                        } else println("Unknown variable")
                        continue
                    }
                    4 -> {
                        val tempList = equation.replace(" ", "").split("=").toList()
                        if (storesVariables.containsKey(tempList[0])) {
                            storesVariables[tempList[0]] = tempList[1].toBigInteger()
                        } else storesVariables[tempList[0]] = tempList[1].toBigInteger()
                        // does not account for "a = 5 + 2" for example, needs changing.
                        continue
                    }
                    5 -> {
                        println("Invalid assignment")
                        continue
                    }
                }

                val equationTransformed = convertLetters(equation, storesVariables)

                if (equationTransformed.first() == '/') {
                    println("Unknown Command")
                    continue
                }
                if (checkValidity(equationTransformed)) {
                    println("Invalid Expression")
                    continue
                }

                val updatedEquation = prepCalculateSum(equationTransformed)
                newCalculateSum(updatedEquation)
                // calls function to calculate the sum of the equation.
            }
        }

    }
}
