Python применяет три режима буферизации для stdout/stderr в зависимости от типа потока:

- **Line buffered** (по строкам): В интерактивном терминале — вывод сразу после \n (как в `python main.py` локально).[](https://realpython.com/python-flush-print-output/)​
	
- **Fully buffered** (полная): В pipe (Docker logs читает через pipe) — собирает ~4096 байт или таймаут (~5 сек), затем сбрасывает. Ваш print ~50 байт/строка накапливался до лимита или Ctrl+C.[](https://stackoverflow.com/questions/39486327/stdout-being-buffered-in-docker-container)​
    
- **Unbuffered** (без): Немедленный вывод — то, что нужно для Docker.[](https://realpython.com/python-flush-print-output/)​

В вашем случае: `docker run --rm` → pipe → full buffering → логи ждали "полной отработки" буфера или KeyboardInterrupt.