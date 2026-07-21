# WSL-ისა და Ubuntu-ს დაყენება

WSL — Windows Subsystem for Linux — საშუალებას გვაძლევს Windows-ში გამოვიყენოთ Linux/Ubuntu გარემო ვირტუალური მანქანის ცალკე მართვის გარეშე.

## მოთხოვნები

- Windows 10 ან Windows 11;
- Administrator უფლებები;
- ინტერნეტთან კავშირი.

## 1. PowerShell-ის Administrator რეჟიმში გახსნა

Windows Start მენიუში მოძებნეთ `PowerShell`, დააჭირეთ მარჯვენა ღილაკს და აირჩიეთ **Run as administrator**.

შემდეგი ბრძანება გაუშვით **Windows PowerShell-ში**, არა Ubuntu-ში:

```powershell
wsl --install -d Ubuntu
```

თუ Windows გადატვირთვას მოგთხოვთ, გადატვირთეთ კომპიუტერი.

## 2. Ubuntu-ს პირველი გაშვება

Start მენიუდან გახსენით **Ubuntu**. პირველი გაშვებისას სისტემა მოგთხოვთ:

- Linux username-ს;
- Linux password-ს.

პაროლის აკრეფისას ეკრანზე სიმბოლოები არ გამოჩნდება. ეს ნორმალურია. აკრიფეთ პაროლი და დააჭირეთ `Enter`.

## 3. Ubuntu-ს განახლება

შემდეგი ბრძანებები გაუშვით **Ubuntu/WSL ტერმინალში**:

```bash
sudo apt update
sudo apt upgrade -y
```

`sudo`-ს გამოყენებისას Ubuntu მოგთხოვთ Linux პაროლს.

## 4. WSL ვერსიის შემოწმება

Windows PowerShell-ში გაუშვით:

```powershell
wsl --list --verbose
```

სასურველი შედეგია, რომ Ubuntu იყენებდეს WSL 2-ს:

```text
NAME      STATE     VERSION
Ubuntu    Running   2
```

თუ Ubuntu იყენებს WSL 1-ს, PowerShell-ში გაუშვით:

```powershell
wsl --set-version Ubuntu 2
```

## 5. Ubuntu-ს გაშვება PowerShell-იდან

```powershell
wsl
```

Ubuntu-ს დასახურად გამოიყენეთ:

```bash
exit
```

## 6. WSL-ის სრული გათიშვა

თუ WSL გაჭედილია, Windows PowerShell-ში გამოიყენეთ:

```powershell
wsl --shutdown
```

შემდეგ Ubuntu თავიდან გახსენით.

## 7. სისტემის ინფორმაციის შემოწმება

Ubuntu-ში:

```bash
uname -a
cat /etc/os-release
```

ეს ბრძანებები აჩვენებს Linux kernel-სა და Ubuntu-ს ვერსიას.
