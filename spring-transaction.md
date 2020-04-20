1.@EnableTransactionManagement 开户事务控制, service结合@Transactional即可

2.service增加注解@Transactional(rollbackFor = {Exception.class})进行控制

3.捕获异常增加回滚操作

try{

}catch(Exception e){

TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();//代码控制回滚

...

} 
