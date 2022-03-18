# Auwee
pragma solidity ^0.4.24;

// ----------------------------------------------------------------------------
// Sample token contract // ตัวอย่างสัญญาโทเค็น
//
// Symbol        : {{Auwee}} // สัญลักษณ์ หรือใส่ชื่อเหรียญที่เราต้องการ
// Name          : {{Auwee Token}} // ชื่อเหรียญ
// Total supply  : {{1000000}} // ราคาเหรียญ
// Decimals      : {{8}} // ทศนิยม
// Owner Account : {{0x61a8793eEf59F4150f6F0ADA12e6A4538F4947Eb}} // บัญชีเจ้าของ ให้
// คัดลอกที่ METAMASK ตรง Account
// Enjoy.
//
// (c) by Juan Cruz Martinez 2020. MIT Licence.
// ----------------------------------------------------------------------------
// คนเขียน Juan Cruz Martinez ใบอนุญาต MIT

// ----------------------------------------------------------------------------
// Lib: Safe Math // ความปลอดภัยคณิตศาสตร์
// ----------------------------------------------------------------------------
contract SafeMath {

    function safeAdd(uint a, uint b) public pure returns (uint c) {
       // ผลตอบแทนสาธารณะที่บริสุทธิ์
        c = a + b;
        require(c >= a);
    }

    function safeSub(uint a, uint b) public pure returns (uint c) {
        require(b <= a);
        c = a - b;
        // ต้องการ (c >= a);
    }

    function safeMul(uint a, uint b) public pure returns (uint c) {
        // ผลตอบแทนสาธารณะที่บริสุทธิ์
        c = a * b;
        require(a == 0 || c / a == b);
        // ต้องการ (a == 0 || c / a == b);
    }

    function safeDiv(uint a, uint b) public pure returns (uint c) {
       // ฟังก์ชั่น safeDiv (uint a, uint b) ผลตอบแทนสาธารณะที่บริสุทธิ์
        require(b > 0);// ความต้องการ
        c = a / b;
    }
}


/**  // อินเทอร์เฟซ
ERC Token Standard #20 Interface
https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md
*/ // สัญญา ERC20อินเทอร์เฟซ
contract ERC20Interface {
    function totalSupply() public constant returns (uint); // ฟังก์ชั่น และผลตอบแทนคงที่สาธารณะ
    function balanceOf(address tokenOwner) public constant returns (uint balance); // ฟังก์ชั่น (ที่อยู่)
    //ผลตอบแทนคงที่สาธารณะ (ยอดคงเหลือ)
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining);
    // ค่าเผื่อการท างาน (ที่อยู่ tokenOwner, ที่อยู่ของผู้รับเงิน) ผลตอบแทนคงที่สาธารณะ(คงเหลืออยู่);
    function transfer(address to, uint tokens) public returns (bool success);
    // การถ่ายโอนฟังก์ชัน (ที่อยู่, โทเค็น uint) ผลตอบแทนสาธารณะ (ความส าเร็จของบูล)
    function approve(address spender, uint tokens) public returns (bool success);
    // ฟังก์ชั่นtransferFrom (ที่อยู่จาก, ที่อยู่ไปยัง, โทเค็น uint) ผลตอบแทนสาธารณะ (ความส าเร็จของบูล);
    function transferFrom(address from, address to, uint tokens) public returns (bool success);
    //ฟังก์ชั่น transferFrom(ที่อยู่จาก, ที่อยู่ไปยัง, โทเค็น uint) ผลตอบแทนสาธารณะ (ความส าเร็จของบูล);
    event Transfer(address indexed from, address indexed to, uint tokens); // การโอนเหตุการณ์ (ที่อยู่ที่จัดท าดัชนีจาก ที่อยู่ที่จัดท าดัชนีไปยัง โทเค็น uint)
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens); // การอนุมัติเหตุการณ์ (ที่อยู่เจ้าของโทเค็นที่จัดท าดัชนี, ผู้ใช้จ่ายที่จัดท าดัชนีที่อยู่ที่, โทเค็น uint);
}

// ฟังก์ชั่นสัญญาเพื่อรับการอนุมัติและด าเนินการฟังก์ชั่นในการโทรครั้งเดียวยืมจาก MiniMeToken
/**
Contract function to receive approval and execute function in one call
Borrowed from MiniMeToken
*/
contract ApproveAndCallFallBack {
    function receiveApproval(address from, uint256 tokens, address token, bytes data) public;
//ฟังก์ชัน รับการอนุมัติ (ที่อยู่จาก, โทเค็น uint256, โทเค็นที่อยู่, ข้อมูลไบต์) สาธารณะ;
}

/**
ERC20 Token, with the addition of symbol, name and decimals and assisted token transfers
// โทเค็น ERC20 พร้อมการเพิ่มสัญลักษณ์ ชื่อ และทศนิยมและการโอนโทเค็นช่วยเหลือ
*/
contract FFFToken is ERC20Interface, SafeMath { // สัญญา
    string public = "Auwee"; // ชื่อเหรียญ
    string public = "Auwee Token"; // ชื่อ โทเคน
    uint8 public  = "8"; // ทศนิยม
    uint public  = "1000000y; //ราคาเหรียญ
    mapping(address => uint) = "0x61a8793eEf59F4150f6F0ADA12e6A4538F4947Eb"; // เชื่อมต่อกับกระเป๋าตังค์เรา
    mapping(address => mapping(address => uint)) = "0x61a8793eEf59F4150f6F0ADA12e6A4538F4947Eb"; // ที่อยู่กระเป๋าเรา


    // ------------------------------------------------------------------------
    // Constructor // ตัวสร้าง
    // ------------------------------------------------------------------------
    constructor() public { // ตัวสร้าง () สาธารณะ
        symbol = "FFF"; // ชื่อเหรียญ
        name = "oil Token"; // ชื่อ โทเค็น ของเรา
        decimals = 4;
        _totalSupply = 1000000; // ราคาเหรียญ
        balances[0xcFCA6a8ED30955EbB1401144fe95ad708D7BEF9A] = _totalSupply; // บัญชีเจ้าของให้คัดลอกที่ METAMASK ตรง Account
        emit Transfer(address(0), 0xcFCA6a8ED30955EbB1401144fe95ad708D7BEF9A, _totalSupply); // โอนที่อยู่ โดยให้คัดลอกที่ METAMASK ตรง Account
    }


    // ------------------------------------------------------------------------
    // Total supply // อุปทานทั้งหมด
    // ------------------------------------------------------------------------
    function totalSupply() public constant returns (uint) { // ฟังก์ชั่น TotalSupply () ผลตอบแทนคงที่สาธารณะ (uint)
        return _totalSupply  - balances[address(0)];
    }
    // จำนวนเหรียญที่ถูกสร้างขึ้น

    // ------------------------------------------------------------------------
    // Get the token balance for account tokenOwner // รับยอดโทเค็นส าหรับบัญชี token Owner
    // ------------------------------------------------------------------------
    function balanceOf(address tokenOwner) public constant returns (uint balance) { // ฟังก์ชั่น ( ที่อยู่) ผลตอบแทนคงที่สาธารณะ ยอดคงเหลือ
        return balances[tokenOwner]; // ยอดคงเหลือ
    }


    // ------------------------------------------------------------------------
    // Transfer the balance from token owner's account to to account // โอนยอดคงเหลือจากบัญชีของเจ้าของโทเค็นไปยังบัญชี
    // - Owner's account must have sufficient balance to transfer // - บัญชีของเจ้าของต้องมียอดเงินเพียงพอในการโอน
    // - 0 value transfers are allowed // - อนุญาตให้โอนค่า 0 ได้
    // ------------------------------------------------------------------------
    function transfer(address to, uint tokens) public returns (bool success) { // การถ่ายโอนฟังก์ชัน (ที่อยู่, โทเค็น uint) ผลตอบแทนสาธารณะ (ความสำเร็จของบูล)
        balances[msg.sender] = safeSub(balances[msg.sender], tokens); // ยอดคงเหลือ [msg.sender]= safeSub (ยอดคงเหลือ [msg.sender], โทเค็น)
        balances[to] = safeAdd(balances[to], tokens); // ยอดคงเหลือ [ถึง] = ปลอดภัยเพิ่ม (ยอดคงเหลือ[ถึง] โทเค็น)
        emit Transfer(msg.sender, to, tokens); // ปล่อยการโอน
        return true; // คืนค่าจริง
    }


    // ------------------------------------------------------------------------
    // Token owner can approve for spender to transferFrom(...) tokens
    // from the token owner's account // จากบัญชีเจ้าของโทเค็น
    //
    // https://github.com/ethereum/EIPs/blob/master/EIPS/eip-20-token-standard.md
    // recommends that there are no checks for the approval double-spend attack // แนะนำว่าไม่มีการตรวจสอบการอนุมัติ
    // as this should be implemented in user interfaces  // เนื่องจากสิ่งนี้ควรนำไปใช้ในส่วนต่อประสานผู้ใช้
    // ------------------------------------------------------------------------
    function approve(address spender, uint tokens) public returns (bool success) { // ฟังก์ชั่นอนุมัติ(ที่อยู่ใช้จ่าย, โทเค็น uint) ผลตอบแทนสาธารณะ (ความส าเร็จของบูล)
        allowed[msg.sender][spender] = tokens; // อนุญาต[msg.sender] [ใช้จ่าย] = โทเค็น
        emit Approval(msg.sender, spender, tokens); // ปล่อยการอนุมัติ
        return true;
    }


    // ------------------------------------------------------------------------
    // Transfer tokens from the from account to the to account // โอนโทเค็นจากบัญชีจากบัญชีไปยังบัญชี
    // 
    // The calling account must already have sufficient tokens approve(...)-d
    // for spending from the from account and // สำหรับการใช้จ่ายจากบัญชี
    // - From account must have sufficient balance to transfer // - จากบัญชีต้องมียอดเงินเพียงพอในการโอน
    // - Spender must have sufficient allowance to transfer // - ผู้ใช้จ่ายต้องมีเบี้ยเลี้ยงเพียงพอในการโอน
    // - 0 value transfers are allowed // - อนุญาตให้โอนค่า 0 ได้
    // ------------------------------------------------------------------------
    function transferFrom(address from, address to, uint tokens) public returns (bool success) { //ฟังก์ชั่น transferFrom(ที่อยู่จาก,ที่อยู่,โทเค็น uint) ผลตอบแทนสาธารณะ (ความสำเร็จของบูล)
        balances[from] = safeSub(balances[from], tokens); // ยอดคงเหลือ[จาก] = safeSub(ยอดคงเหลือ[จาก], โทเค็น);
        allowed[from][msg.sender] = safeSub(allowed[from][msg.sender], tokens); // อนุญาต[จาก][msg.sender] = safeSub(อนุญาต[จาก][msg.sender], โทเค็น);
        balances[to] = safeAdd(balances[to], tokens);
        emit Transfer(from, to, tokens);
        return true;
    }


    // ------------------------------------------------------------------------
    // Returns the amount of tokens approved by the owner that can be
    // transferred to the spender's account
    // ------------------------------------------------------------------------
    function allowance(address tokenOwner, address spender) public constant returns (uint remaining) {
        return allowed[tokenOwner][spender];
    }


    // ------------------------------------------------------------------------
    // Token owner can approve for spender to transferFrom(...) tokens
    // from the token owner's account. The spender contract function
    // receiveApproval(...) is then executed
    // ------------------------------------------------------------------------
    function approveAndCall(address spender, uint tokens, bytes data) public returns (bool success) {
        allowed[msg.sender][spender] = tokens;
        emit Approval(msg.sender, spender, tokens);
        ApproveAndCallFallBack(spender).receiveApproval(msg.sender, tokens, this, data);
        return true;
    }


    // ------------------------------------------------------------------------
    // Don't accept ETH
    // ------------------------------------------------------------------------
    function () public payable {
        revert();
    }
}
