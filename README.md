import backtrader as bt

class MeanReversion(bt.Strategy):
    params = (
        ('period', 20),
        ('devfactor', 2),
    )

    def __init__(self):
        self.sma = bt.indicators.SMA(self.data, period=self.p.period)
        self.stddev = bt.indicators.StdDev(self.data, period=self.p.period)
        self.upperband = self.sma + self.stddev * self.p.devfactor
        self.lowerband = self.sma - self.stddev * self.p.devfactor

    def next(self):
        if self.data.close[0] > self.upperband[0]:
            self.sell()
        elif self.data.close[0] < self.lowerband[0]:
            self.buy()

# ... (rest of the backtesting setup)
