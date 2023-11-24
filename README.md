import requests
import os

def vulnerable_function(username, password):
    url = "https://example.com/api/login"
    data = {
        "username": username,
        "password": password
    }
    response = requests.post(url, data=data)
    if response.status_code == 200:
       return response.json()
    else:
       return None

def vulnerable_function2(path):
    filename = os.path.basename(path)
    if filename.endswith(".py"):
        return open(path, "r").read()
    else:
        return None

def vulnerable_function3(payload):
    return pickle.loads(payload)

def main():
    username = input("Introduce tu nombre de usuario: ")
    password = input("Introduce tu contraseña: ")
    data = vulnerable_function(username, password)
    if data is not None:
        print("El usuario {} se ha autenticado correctamente".format(data["username"]))
        print("El token de sesión es: {}".format(data["token"]))
        vulnerable_function2("/etc/passwd")
        vulnerable_function3("""
            print("Este código se ha ejecutado con privilegios elevados")
        """.encode())

if __name__ == "__main__":
    main()
