Node Map of Thomson Reuters Business Classification (TRBC)
==========================================================

This library just aims to enable lookups in the reference table of Morningstar as published on their [website](http://thomsonreuters.com/content/dam/openweb/documents/excel/tr-com-financial/trbc-industry-descriptions.xls) in an easy way by building a NodeJS module containing a JSON map and some methods to explore it.

## Installation

```shell
npm install thomson-reuters-trbc
```

## Usage

```javascript
var TRBC = require('thomson-reuters-trbc');

TRBC.all()
//=> [ { name: 'ENERGY', code: 50, type: 'Economic Sector', permId: 219 }, ...]

TRBC.search('finance');
/*=> [ { code: 5510105014,
         type: 'Activity',
         economicSector: 55,
         businessSector: 5510,
         industryGroup: 551010,
         industry: 55101050,
         name: 'Factoring',
         permId: 1631,
         description: 'The Factoring activity consists of companies engaged in providing a financial transaction whereby a business job sells its accounts receivable (i.e., invoices) to a third party (called a factor) in order to get in exchange for immediate money to finance business.',
         score: 1 },
       { code: 5510201015,
         type: 'Activity',
         economicSector: 55,
         businessSector: 5510,
         industryGroup: 551020,
         industry: 55102010,
         name: 'Merchant Banks',
         permId: 1638,
         description: 'The Merchant Bank activity consists of banks engaged mostly in (but is not limited to) international finance, long-term loans for companies and underwriting. Merchant banks do not provide regular banking services to the general public. The distinction between an investment bank and a merchant bank is that a merchant bank invests its own capital in a client company whereas an investment bank purely distributes (and trades) the securities of that company',
         score: 1 } ]
*/

TRBC.find('5720101014');
/*=> { code: 5720101014,
       type: 'Activity',
       economicSector: 57,
       businessSector: 5720,
       industryGroup: 572010,
       industry: 57201010,
       name: 'Testing Services',
       permId: 1794,
       description: 'The Testing Services activity consists of companies engaged in providing a set of principles and methodologies for designing and developing software in the form of interoperable services. This activity includes services that are business functionalities that are built as software components (discrete pieces of code and/or data structures) that can be reused for different purposes.' }
*/

TRBC.above('5720101014');
/*=> [ { code: 5720101014,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Testing Services',
         permId: 1794,
         description: 'The Testing Services activity consists of companies engaged in providing a set of principles and methodologies for designing and developing software in the form of interoperable services. This activity includes services that are business functionalities that are built as software components (discrete pieces of code and/or data structures) that can be reused for different purposes.' },
       { code: 57201010,
         type: 'Industry',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         name: 'IT Services & Consulting',
         permId: 290 },
       { code: 572010,
         type: 'Industry Group',
         economicSector: 57,
         businessSector: 5720,
         name: 'Software & IT Services',
         permId: 172 },
       { code: 5720,
         type: 'Business Sector',
         economicSector: 57,
         name: 'Software & IT Services',
         permId: 171 },
       { name: 'TECHNOLOGY',
         code: 57,
         type: 'Economic Sector',
         permId: 278 } ]
*/

TRBC.below('57201010');
/*=> [ { code: 57201010,
         type: 'Industry',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         name: 'IT Services & Consulting',
         permId: 290 },
       { code: 5720101010,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Other IT Services & Consulting',
         permId: 1790,
         description: 'The Other IT Services & Consulting activity consists of companies engaged in providing customized software development, ISP providers, online support services, online database management, interactive data access and auctioning services.' },
       { code: 5720101011,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Computer Programming',
         permId: 1791,
         description: 'The Computer Programming activity consists of companies engaged in providing customized software development services. This activity excludes specialized testing services classified under Testing Services activity.' },
       { code: 5720101012,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Computer Training',
         permId: 1792,
         description: 'The Computer Training activity consists of companies engaged in providing the knowledge and ability to use computers and related technology efficiently, with a range of skills covering levels from elementary use to programming and advanced problem solving.' },
       { code: 5720101013,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Technology Consulting & Outsourcing Services',
         permId: 1793,
         description: 'The Technology Consulting & Outsourcing Services activity consists of companies engaged in technology advising, computer facility management service, and system integration. This activity also includes outsourcing services, such as information technology outsourcing wherein a company outsources computer or Internet related work, such as programming, from other companies.' },
       { code: 5720101014,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Testing Services',
         permId: 1794,
         description: 'The Testing Services activity consists of companies engaged in providing a set of principles and methodologies for designing and developing software in the form of interoperable services. This activity includes services that are business functionalities that are built as software components (discrete pieces of code and/or data structures) that can be reused for different purposes.' },
       { code: 5720101015,
         type: 'Activity',
         economicSector: 57,
         businessSector: 5720,
         industryGroup: 572010,
         industry: 57201010,
         name: 'Cloud Computing Services',
         permId: 1795,
         description: 'The Cloud Computing Services activity consists of companies engaged in the delivery of computing as a service rather than a product, whereby shared resources, software, and information are provided to computers and other devices as a utility (like the electricity grid) over a network (typically the Internet).' } ]
*/
```

## Dataset Indexing

In order to enable keyword-based searches on the TRBC dataset, we use a reverse index stored in ``/data/data-index.json``. The index is generated by tokenizing relevant properties on TRBC classifications and storing them in an index object keyed by the tokenized text. The index relates keywords to scores that represent the likelihood that a specific keyword is related to a TRBC classification.

In order to rebuild the index run the following command from the project directory:

```shell
$ ./bin/build-index
```

(inspired from @lovehandle/naics-2012 - thanks !)