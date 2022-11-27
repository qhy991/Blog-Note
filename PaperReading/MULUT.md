[[2022-11-16]]

MULUT的finetune操作的目的：
为了减小从SRNET-->LUT由于采样和插值带来的误差。
finetune的策略是把LUT里的数据视作可训练参数，类似LUT 重新索引（re-indexing）