import sys
import pyfiglet
import rich
import argparse
from openai import OpenAI
import subprocess


def main():
    print('This is Py2AnyCode.')
    rich.print(f"[cyan]{pyfiglet.figlet_format('Py2AnyCode', font='slant')}[/cyan]")
    if len(sys.argv) < 2:
        rich.print("[red]请输入参数。[/red]")
        sys.exit(1)
    parser = argparse.ArgumentParser(description="处理传入的文件")
    parser.add_argument('-t', '--type', type=str, required=True, help="转换类型")
    parser.add_argument('-i', '--input', type=str, required=True, help="转换文件路径")
    args = parser.parse_args()
    convert_type = args.type
    convert_file_type = {'c': 'c', 'c++': 'cpp', 'go': 'go', 'java': 'java'}[convert_type]
    compiler = {'go': r'C:\Program Files\Go\bin\go.exe',
                'c++': r'C:\Program Files\JetBrains\CLion 2024.2.2\bin\mingw\bin\g++.exe'}
    if convert_type not in ['c', 'c++', 'go', 'java']:
        rich.print("[red]转换类型错误，请选择 c, c++, go 或 java。[/red]")
        sys.exit(1)

    filename = args.input

    try:
        with open(filename, 'r', encoding='utf-8') as f:
            content = f.readlines()
        client = OpenAI(api_key="Change this", base_url="https://api.deepseek.com")
        messages = [{"role": "user", "content": f'请将以下python代码转换为{convert_type}：\n{'\n'.join(content)}'}]
        response = client.chat.completions.create(
            model="deepseek-reasoner",
            messages=messages,
            stream=False, timeout=500000
        )
        result = response.choices[0].message.content.split('```')
        if len(result) == 1:
            result = result[0]
        else:
            result = result[1]
        final_file_name = filename.replace('.py', f'.{convert_file_type}')
        with open(final_file_name, 'w', encoding='utf-8') as f:
            f.write(result)
        rich.print(f'[green]转换完成！见{final_file_name}[/green]')
        command = [compiler[convert_type], final_file_name]
        process = subprocess.Popen(
            command,
            stdout=subprocess.PIPE,
            stderr=subprocess.STDOUT,
            text=True,
            shell=True
        )
        for line in process.stdout:
            print(line, end='')
        process.wait()
    except FileNotFoundError:
        rich.print(f"[red]文件 '{filename}' 未找到。[red]")
    except Exception as e:
        rich.print(f"[red]发生出错：{e}[red]")
        raise e


if __name__ == "__main__":
    main()
