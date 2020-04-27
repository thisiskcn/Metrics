# -*- coding: utf-8 -*-
from selenium import webdriver
import unittest
import time
import re
import json


class Assignment(unittest.TestCase):
    def setUp(self):
        self.driver = webdriver.Chrome("chromedriver.exe")
        self.driver.implicitly_wait(30)
        self.verificationErrors = []

    def test_assignment(self):
        driver = self.driver
        total = {}

        # to create output file and append name and duration
        with open("output.txt", "w") as json_file:
            for result in range(10):
                driver.get("https://en.wikipedia.org/wiki/Software_metric")
                result = driver.execute_script("return window.performance.getEntries()")
                print(result)
                print(len(result))

                for current in result:
                    print(dir(current))
                    print(current.keys())
                    url = current['name']
                    current_list = total.get(url, [])
                    current_list.append(current["duration"])
                    total[url] = current_list
                    print("current list" + str(current_list))
                    print("total" + str(total))
                    print(current['name'])
                    print(current['duration'])
                    print(current.keys())
                    json_file.write(f"{current['name']}, {current['duration']}\n")

        # to create csv output file with calculation of average duration
        with open("csvoutput.csv", "w") as csv_file:
            for key, value in total.items():
                avg = sum(value) / len(total)
                csv_file.write(f"{key}, {avg}\n")

        print(total)

        # to create a json output file and prettify it
        with open("json_output" + ".json", "w", encoding="utf-8") as file:
            json.dump(result, file, ensure_ascii=False, indent=4)

    def tearDown(self):
        self.driver.quit()
        self.assertEqual([], self.verificationErrors)


if __name__ == "__main__":
    unittest.main()
