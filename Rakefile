require 'byebug'
require 'rubygems'
require 'json'
require 'faraday'

# be rake local erip_stack

task :local do
  SHOP_ID           = '1'
  SHOP_PASSWORD     = 'f2573d272b1076354b95099a513455b5152ea67888bc5d4e45ac51147ac2fc0c'
  URL               = 'http://192.168.66.62:3005/'
end

task :erip_stack do
  send_erip_stack
end

def parse_response(response)
  JSON.parse(response)
end

def send_erip_stack
  # --- Read Account number from file to increase it automatically
  emulation_number   =  File.read("erip/account_number_counter.txt").to_i
  emulation_number   += 1

  # --- Initialize account_number and request id
  amount         = 19000
  account_number = emulation_number.to_s
  request_id     = '1357' << account_number
  transaction_id = account_number
  order_id       = "order_n_#{account_number}"
  
  # --- Create ERIP PAYMENT  
  file_name       = "erip/parent_erip.json"
  outdata   = File.read(file_name).gsub(/\"account_number\":\"\d+\"/, "\"account_number\":\"#{account_number}\"")
  outdata   = outdata.gsub(/\"amount\":\d+/, "\"amount\":#{amount}")
  outdata   = outdata.gsub(/\"order_id\":\"\w+\"/, "\"order_id\":\"#{order_id}\"")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/json; charset=utf-8" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/payments`
  puts response

  # --- SEND SERVICE INFO EMULATION
  file_name = "erip/serviceinfo.xml"
  outdata   = File.read(file_name).gsub(/<RequestId>\d+<\/RequestId>/, "<RequestId>#{request_id}<\/RequestId>")
  outdata   = outdata.gsub(/<PersonalAccount>\d+<\/PersonalAccount>/, "<PersonalAccount>#{account_number}<\/PersonalAccount>")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/xml; charset=windows-1251" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/notifications/erip`
  puts response.force_encoding('windows-1251').encode('utf-8')


  # --- SEND TRANSACTION START INFO EMULATION  
  file_name = "erip/startpay.xml"
  outdata   = File.read(file_name).gsub(/<RequestId>\d+<\/RequestId>/, "<RequestId>#{request_id}<\/RequestId>")
  outdata   = outdata.gsub(/<PersonalAccount>\d+<\/PersonalAccount>/, "<PersonalAccount>#{account_number}<\/PersonalAccount>")
  outdata   = outdata.gsub(/<TransactionId>\d+<\/TransactionId>/, "<TransactionId>#{transaction_id}<\/TransactionId>")
  outdata   = outdata.gsub(/<Amount>\d+<\/Amount>/, "<Amount>#{amount}<\/Amount>")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/xml; charset=windows-1251" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/notifications/erip`
  puts response.force_encoding('windows-1251').encode('utf-8')

  # --- SEND TRANSACTION FINISH INFO EMULATION
  file_name = "erip/finishpay.xml"
  outdata   = File.read(file_name).gsub(/<RequestId>\d+<\/RequestId>/, "<RequestId>#{request_id}<\/RequestId>")
  outdata   = outdata.gsub(/<PersonalAccount>\d+<\/PersonalAccount>/, "<PersonalAccount>#{account_number}<\/PersonalAccount>")
  outdata   = outdata.gsub(/<TransactionId>\d+<\/TransactionId>/, "<TransactionId>#{transaction_id}<\/TransactionId>")
  outdata   = outdata.gsub(/<ServiceProvider_TrxId>\w+<\/ServiceProvider_TrxId>/, "<ServiceProvider_TrxId>#{order_id}<\/ServiceProvider_TrxId>")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/xml; charset=windows-1251" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/notifications/erip`
  puts response.force_encoding('windows-1251').encode('utf-8')


  # --- SEND TRANSACTION START STORN EMULATION
  emulation_number += 1
  request_id     = '1357' << emulation_number.to_s

  file_name = "erip/startstorn.xml"
  
  outdata   = File.read(file_name).gsub(/<RequestId>\d+<\/RequestId>/, "<RequestId>#{request_id}<\/RequestId>")
  outdata   = outdata.gsub(/<PersonalAccount>\d+<\/PersonalAccount>/, "<PersonalAccount>#{account_number}<\/PersonalAccount>")
  outdata   = outdata.gsub(/<TransactionId>\d+<\/TransactionId>/, "<TransactionId>#{transaction_id}<\/TransactionId>")
  outdata   = outdata.gsub(/<ServiceProvider_TrxId>\w+<\/ServiceProvider_TrxId>/, "<ServiceProvider_TrxId>#{order_id}<\/ServiceProvider_TrxId>")
  outdata   = outdata.gsub(/<Amount>\d+<\/Amount>/, "<Amount>#{amount}<\/Amount>")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/xml; charset=windows-1251" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/notifications/erip`
  puts response.force_encoding('windows-1251').encode('utf-8')


  # --- SEND TRANSACTION STORN RESULT EMULATION
  file_name = "erip/finishstorn.xml"
  
  outdata   = File.read(file_name).gsub(/<RequestId>\d+<\/RequestId>/, "<RequestId>#{request_id}<\/RequestId>")
  outdata   = outdata.gsub(/<PersonalAccount>\d+<\/PersonalAccount>/, "<PersonalAccount>#{account_number}<\/PersonalAccount>")
  outdata   = outdata.gsub(/<TransactionId>\d+<\/TransactionId>/, "<TransactionId>#{transaction_id}<\/TransactionId>")
  outdata   = outdata.gsub(/<ServiceProvider_TrxId>\w+<\/ServiceProvider_TrxId>/, "<ServiceProvider_TrxId>#{order_id}<\/ServiceProvider_TrxId>")
  outdata   = outdata.gsub(/<Amount>\d+<\/Amount>/, "<Amount>#{amount}<\/Amount>")
  File.open(file_name, 'w') { |out| out << outdata }
  response = `curl -H "Content-type: application/xml; charset=windows-1251" -u #{SHOP_ID}:#{SHOP_PASSWORD} -d @#{file_name} #{URL}/notifications/erip`
  puts response.force_encoding('windows-1251').encode('utf-8')
  
  File.open(  file_name = "erip/account_number_counter.txt", 'w') { |out| out << emulation_number.to_s }
end
