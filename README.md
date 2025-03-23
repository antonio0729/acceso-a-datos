# acceso-a-datos
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
