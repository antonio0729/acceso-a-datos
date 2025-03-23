# Gestion de banco
Realiza 
1. Clase TarjetaCredito: Debe tener los siguientes atributos: numero_tarjeta: Representa el número de la tarjeta. saldo_pendiente: Representa el saldo que el usuario debe pagar. Métodos: validar_tarjeta(numero) : Un método estático que valida si un número de tarjeta es válido utilizando el algoritmo de Luhn . El método debe retornar True si el número es válido y False en caso contrario. consultar_saldo_pendiente() : Retorna el saldo pendiente de la tarjeta. pagar(cantidad) : Reduce el saldo pendiente de la tarjeta si la cantidad a pagar es válida. 

2. Clase CuentaBancaria: Debe tener los siguientes atributos privados: __saldo: Representa el saldo disponible en la cuenta. __titular: Representa el nombre del titular de la cuenta. tarjeta: Una instancia de la clase TarjetaCredito asociada a la cuenta. Métodos: depositar(cantidad) : Deposita dinero en la cuenta si la tarjeta asociada es válida. retirar(cantidad) : Retira dinero de la cuenta si hay suficiente saldo y la tarjeta asociada es válida. consultar_saldo() : Retorna el saldo actual de la cuenta. consultar_titular() : Retorna el nombre del titular de la cuenta. realizar_pago_tarjeta(cantidad) : Transfiere dinero desde la cuenta bancaria al saldo pendiente de la tarjeta, siempre que la tarjeta sea válida y haya suficiente saldo en la cuenta. 

3. Validación de Números de Tarjeta: Antes de realizar cualquier operación (depósito, retiro o pago), se debe validar que el número de tarjeta asociado sea válido utilizando el método estático validar_tarjeta. Si el número de tarjeta no es válido, se debe mostrar un mensaje de error y cancelar la operación. 4. Ejemplo de Uso: Crea una cuenta bancaria con un titular, un número de tarjeta válido y un saldo inicial. Configura un saldo pendiente inicial en la tarjeta. Realiza operaciones como consultar saldos, realizar depósitos, retiros y pagos. Maneja errores adecuadamente (por ejemplo, saldo insuficiente o número de tarjeta inválido).


class TarjetaCredito:
    def __init__(self, numero_tarjeta, saldo_pendiente):
        self.numero_tarjeta = numero_tarjeta
        self.saldo_pendiente = saldo_pendiente

    @staticmethod
    def validar_tarjeta(numero):
        if not isinstance(numero, str):
            print("Error: El numero de tarjeta debe ser una cadena.")
            return False

        numero_invertido = list(numero)
        numero_invertido.reverse()

        suma_total = 0
        for i, digito in enumerate(numero_invertido):
            digito = int(digito)
            if i % 2 != 0:  
                digito *= 2
                if digito > 9:
                    digito -= 9
            suma_total += digito

        return suma_total % 10 == 0

    def consultar_saldo_pendiente(self):
        return self.saldo_pendiente

    def pagar(self, cantidad):
        if cantidad > 0 and cantidad <= self.saldo_pendiente:
            self.saldo_pendiente -= cantidad
            print(f"\nPago realizado correctamente.\nSaldo pendiente: {self.saldo_pendiente}\n")
        else:
            print("\nCantidad invalida o excede el saldo pendiente.\n")

class CuentaBancaria:
    def __init__(self, titular, saldo_inicial, tarjeta):
        self.titular = titular
        self.saldo = saldo_inicial
        self.tarjeta = tarjeta

    def depositar(self, cantidad):
        if not TarjetaCredito.validar_tarjeta(self.tarjeta.numero_tarjeta):
            print("\nError: Numero de tarjeta invalido.\n")
            return

        if cantidad > 0:
            self.saldo += cantidad
            print(f"\nDeposito realizado correctamente.\n\nSaldo actual: {self.saldo}\n")
        else:
            print("\nCantidad a depositar invalida.\n")

    def retirar(self, cantidad):
        if not TarjetaCredito.validar_tarjeta(self.tarjeta.numero_tarjeta):
            print("\nError: Numero de tarjeta invalido.\n")
            return

        if cantidad > 0 and cantidad <= self.saldo:
            self.saldo -= cantidad
            print(f"\nRetiro realizado correctamente.\n\nSaldo actual: {self.saldo}\n")
        elif cantidad > self.saldo:
            print("\nError: No hay suficiente saldo para realizar el retiro.\n")
        else:
            print("\nCantidad invalida.\n")

    def realizar_pago_tarjeta(self, cantidad):
        if not TarjetaCredito.validar_tarjeta(self.tarjeta.numero_tarjeta):
            print("\nError: Numero de tarjeta invalido.\n")
            return

        if cantidad > 0 and cantidad <= self.saldo:
            self.saldo -= cantidad
            self.tarjeta.pagar(cantidad)
            print(f"\nPago a tarjeta realizado.\n\nSaldo actual en cuenta: {self.saldo}\n")
        else:
            print("\nSaldo insuficiente o cantidad invalida.\n")

# Ejemplo de uso
if __name__ == "__main__":
    tarjeta_invalida = TarjetaCredito("1234567890321456", 5000)

    if TarjetaCredito.validar_tarjeta(tarjeta_invalida.numero_tarjeta):
        print(f"\nEl numero de tarjeta {tarjeta_invalida.numero_tarjeta} es valido.\n")
    else:
        print(f"\nEl numero de tarjeta {tarjeta_invalida.numero_tarjeta} es invalido.\n")

    tarjeta_valida = TarjetaCredito("4539148803436467", 4000)

    if TarjetaCredito.validar_tarjeta(tarjeta_valida.numero_tarjeta):
        print(f"\nEl numero de tarjeta {tarjeta_valida.numero_tarjeta} es valido.\n")
    else:
        print(f"\nEl numero de tarjeta {tarjeta_valida.numero_tarjeta} es invalido.\n")

    cuenta = CuentaBancaria("Antonio Stephens", 1000, tarjeta_valida)

    print(f"\nSaldo en la cuenta: {cuenta.saldo}\n")
    print(f"Saldo pendiente en la tarjeta: {tarjeta_valida.consultar_saldo_pendiente()}\n")

    cuenta.depositar(500)

    cuenta.retirar(300)

    cuenta.realizar_pago_tarjeta(200)

    print(f"\nSaldo en la cuenta despues de las operaciones: {cuenta.saldo}\n")
    print(f"Saldo pendiente en la tarjeta despues del pago: {tarjeta_valida.consultar_saldo_pendiente()}\n")
