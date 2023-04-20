<?php

declare(strict_types=1);

class NumberToWordsGerman
{
    private const TENS = [
        2 => 'zwanzig',
        3 => 'dreißig',
        4 => 'vierzig',
        5 => 'fünfzig',
        6 => 'sechzig',
        7 => 'siebzig',
        8 => 'achtzig',
        9 => 'neunzig',
    ];

    private const SINGLES = [
        0 => '',
        1 => 'ein',
        2 => 'zwei',
        3 => 'drei',
        4 => 'vier',
        5 => 'fünf',
        6 => 'sechs',
        7 => 'sieben',
        8 => 'acht',
        9 => 'neun',
        10 => 'zehn',
        11 => 'elf',
        12 => 'zwölf',
        13 => 'dreizehn',
        14 => 'vierzehn',
        15 => 'fünfzehn',
        16 => 'sechzehn',
        17 => 'siebzehn',
        18 => 'achtzehn',
        19 => 'neunzehn',
    ];

    private const BLANK = ' ';
    private const JOIN = 'und';
    private const ZERO = 'null';
    private const NEGATIVE = 'minus';
    private const SUFFIX_ONE_SINGULAR = 's';
    private const SUFFIX_ONE_MILLION = 'e';

    private function getSuffixByExponent(int $length): string
    {
        switch ($length) {
            case 2:
                return 'hundert';
            case 3:
            case 4:
            case 5:
                return 'tausend';
            case 6:
            case 7:
            case 8:
                return 'Millionen';
            case 9:
            case 10:
            case 11:
                return 'Milliarden';
            case 12:
            case 13:
            case 14:
                return 'Billionen';
            case 15:
            case 16:
            case 17:
                return 'Billiarden';
            default:
                return '';
        }
    }

    private function internalNumberToWord(int $number): string
    {
        if ($number < 20) {
            return (self::SINGLES[$number] . (($number === 1) ? self::SUFFIX_ONE_SINGULAR : ''));
        }

        $exponent = (int)(log10(num: $number));
        if ($exponent < 2) {
            if ($number % 10 == 0) {
                return self::TENS[(int)($number / 10)];
            } else {
                return (self::SINGLES[$number % 10] . self::JOIN . self::TENS[$number / 10]);
            }
        } else {
            $blank = '';
            if ($exponent >= 6) {
                $blank = self::BLANK;
            }
            if ($exponent > 2) {
                $tensPower = pow(num: 10, exponent: ($exponent - ($exponent % 3)));
            } else {
                $tensPower = pow(num: 10, exponent: $exponent);
            }
            $n = (int)($number / $tensPower);
            if ($n === 1) {
                $k = self::SINGLES[$n] . (($exponent >= 6) ? self::SUFFIX_ONE_MILLION : '');
            } else {
                $k = $this->internalNumberToWord($n);
            }
            return ($k . $blank . $this->getSuffixByExponent($exponent)) . $blank . $this->internalNumberToWord($number % $tensPower);
        }
    }

    public function numberToWord(int $number): string
    {
        if ($number === 0) {
            return self::ZERO;
        }
        if ($number === 1) {
            return self::SINGLES[$number] . self::SUFFIX_ONE_SINGULAR;
        }

        return trim((($number < 0) ? self::NEGATIVE . self::BLANK : '') . $this->internalNumberToWord(abs(num: $number)));
    }
}
