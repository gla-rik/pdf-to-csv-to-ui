import camelot
import os
import pandas as pd
import csv

pdf = 'a.pdf'
page = "page_i.csv"
output_csv = "result.csv"


def extract_data():
    df = camelot.read_pdf(pdf, pages='all')
    df[0].to_csv(output_csv)
    with open(output_csv, encoding='utf-8') as text_in:
        row = text_in.readlines()
        del row[0]
        del row[0]
        row[0] = '"НАПРАВЛЕНИЯ","СПЕЦИАЛЬНОСТИ","Кол-во мест для приема","Всего","Вид документа","Согласие"'
    with open(output_csv, 'w', encoding='utf-8') as text_out:
        for i in row:
            j = i.replace('\n', '')
            print(j, file=text_out)

    for i in range(1, len(df)):
        df[i].to_csv(page)
        with open(page, encoding='utf-8') as text_in:
            row = text_in.readlines()
        with open(page, 'w', encoding='utf-8') as text_out:
            print('"НАПРАВЛЕНИЯ","СПЕЦИАЛЬНОСТИ","Кол-во мест для приема","Всего","Вид документа","Согласие"',
                  sep='\n', file=text_out)
            for i in row:
                s = i.replace('\n', '')
                siz = i.count(',')
                if (siz == 4) & (i[0] == '"'):
                    s = ',' + s
                if (0 == siz) & (i[0] == '"'):
                    s = ',' + s
                print(s, file=text_out)
        tables = pd.concat([pd.read_csv(output_csv, sep=','), pd.read_csv(page, sep=',')])
        tables.to_csv(output_csv, index=False)
    os.remove(page)
    info()


def student():
    name = input('Введите ФИО: ')
    with open(output_csv, encoding='utf-8') as file:
        table = csv.DictReader(file)
        for row in table:
            if not row['НАПРАВЛЕНИЯ'] == '':
                code = row['НАПРАВЛЕНИЯ']
                spec = row['СПЕЦИАЛЬНОСТИ'].replace('\n', '')
            if name in row['СПЕЦИАЛЬНОСТИ']:
                print('==================================================')
                print(row['СПЕЦИАЛЬНОСТИ'])
                print('Направление: ', code)
                print('Специальность: ', spec)
                print('Вид документа: ', row['Вид документа'])
                print('Согласие: ', row['Согласие'])
                print('==================================================')


def specialization():
    check = False
    spec = input('Введите направление')
    with open(output_csv, encoding='utf-8') as csvfile:
        reader = csv.DictReader(csvfile)
        for row in reader:
            if not row['НАПРАВЛЕНИЯ'] == '':
                if spec == row['НАПРАВЛЕНИЯ']:
                    print(row['НАПРАВЛЕНИЯ'], row['СПЕЦИАЛЬНОСТИ'], row['Кол-во мест для приема'], row["Всего"],
                          row["Вид документа"], row["Согласие"])
                    check = True
                    spec = None
                else:
                    check = False
                    continue
            elif check:
                print(row['СПЕЦИАЛЬНОСТИ'].replace('\n', ''), end=' ')
                print(row['НАПРАВЛЕНИЯ'], row["Вид документа"], row["Согласие"])
            else:
                check = False

    close = int(input('Выйти из программы или продолжить (1/any key): '))

    if close == 1:
        info()


def info():
    question = int(input('Вывести информацию о СТУДЕНТЕ или о НАПРАВЛЕНИИ (1/2): '))
    if question == 1:
        student()
    elif question == 2:
        specialization()


if os.path.isfile('result.csv'):
    info()
else:
    extract_data()
